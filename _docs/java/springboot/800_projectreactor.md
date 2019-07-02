---
title: Project Reactor
tags: [spring, spring_boot, reactive]
category: spring
summary: "Project Reactor"
---

# Overview

Some notes about Project Reactor (<https://projectreactor.io/>>)

# Examples

Add event listener to button and process events:

~~~java

JButton button = ...;

Flux.create(sink -> button.addActionListener(sink::next))
// transform the ActionEvent to some other type:
.flux.map(...)

// Switch execution from Event Dispatch Thread to worker: 
// "publishOn forces the next operator (and possibly subsequent operators after the next one) 
// to run on a different thread."
.publishOn(Schedulers.parallel())

// do something
.map(...)

// continue in Swing Event Dispatcher
.publishOn(SwingScheduler.create())

// set the new text of the button
.subscribe(button::setText);

~~~

# References

* <https://projectreactor.io/>
