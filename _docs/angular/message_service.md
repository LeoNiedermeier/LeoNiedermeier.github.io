---
title: Simple Message Service
tags: [angular]
category: angular
summary: "Simple Message Service"
---

# Overview

A simple message service should show messages which are created by differnet classes. 
When the router executes a routing, the the currently shown messages should be cleared.

# The Message Class

A message has a text and a type. Both are readonly fields of the `Message` class:

~~~ts
export class Message {

  constructor(readonly type: MessageType, readonly  text: String) {
  }
}

export enum MessageType {
  INFO,
  ERROR,
  CLEAR
}
~~~

Note the "special" type `CLEAR` which is used to indicate that the currently shown messages should be removed 
(the message queue should be cleared).  


# The MessagePanelComponent

The template for the `MessagePanelComponent` is quite simple:

~~~html
<div *ngFor="let message of messages">
    <!-- the default is a bit lengthy do formulate in the case if there are more message types -->
    <!--  use a class binding for the different message types to show -->
    <div class="alert" [class.alert-danger]="message.type == messageType.ERROR"
                       [class.alert-info]="message.type != messageType.ERROR">
        {{message.text}}
    </div>
</div>
~~~

Not the usage of the `messageType` property of `MessagePanelComponent` in order to access an element from the global namespace. 
(Accessing it in a "static" manner like `MessageType.ERROR` does not work 
because the `MessagePanelComponent` has no property `MessageType`. The context against which the expression is resolved is the `MessagePanelComponent`)


The `MessagePanelComponent` manages the list of messages to show:

~~~ts
export class MessagePanelComponent implements OnDestroy {
 // Template can only access elements from the component. Access to other elements is not possible.
  // Therefore we have to add a property in order to access the enum:
  public readonly messageType = MessageType;

  public messages: Message[] = [];

  private subscription: Subscription;

  constructor(messageService: MessageService) {
    this.subscription = messageService.subscribe(m => this.addMessage(m));
  }

  private addMessage(message: Message): void {
    // The "CLEAR" type removes the currently shown messages:
    if (message.type === MessageType.CLEAR) {
      this.messages = [];
    } else {
      this.messages.push(message);
    }
  }
  ngOnDestroy(): void {
    this.subscription.unsubscribe();
  }
}
~~~

# The MessageService

The `MessageService` simply publishes the messages by using a `Subject`:

~~~ts
@Injectable()
export class MessageService implements OnDestroy {
  private readonly messageSubject: Subject<Message> = new Subject<Message>();
  constructor() {
  }

  public publish(message: Message): void {
    this.messageSubject.next(message);
  }

  public subscribe(next?: (value: Message) => void, error?: (error: any) => void, complete?: () => void): Subscription {
    // Currently just delegates to the subject.
    return this.messageSubject.subscribe(next, error, complete);
  }

  ngOnDestroy(): void {
    this.messageSubject.complete();
  }
}
~~~

The `MessageService` has to be regestered a provider.

# Register MessageService as Router Event Listener

In order to remove the messages when a routing is initiated, we register a listener to the `Router.events` in the `AppRoutingModule`:

~~~ts
const routes: Routes = [...];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
  ...
})
export class AppRoutingModule {

  constructor(messageService: MessageService, router: Router) {
    router.events.subscribe(event => {
      // clear the message panel on "page change"
      if (event instanceof NavigationStart) {
        messageService.publish(Message.clear());
      }
    });
  }
}
~~~

# Using the MessageService

The `MessageService` can be injected by Angular in a constuctor and then used in the class

~~~ts
constructor(private messageService: MessageService) {
}

doSomething(...): ... {
    ...
    this.messageService.publish(new Message(MessageType.ERROR, "My error text");
    ...
}
~~~

