---
title: Receive a Call | SIP.js
description: How to enable your WebRTC application to accept calls from peers and third parties by registering a SIP user agent. 
---

# Receive a Call

* TOC
{:toc}


### Setup

We will start with the code that we produced at the end of the [make a call](/guides/make-call/) example.


### Register A SIP User Agent

In order to receive calls we need to create a registered SIP user agent, instead of an anonymous user agent.  Instead of calling the `SIP.UA()` method without any parameters, pass in a configuration object that contains a `uri`, a `ws_servers`, and a `password`.

We will also remove `session.invite()`, so that our app stops making calls.


### Accept a Call

Finally, To accept a call that is being received, catch the `invite` event.  This event is emitted with a session that the `.accept()` method must be called on.

<iframe
  style="width: 100%; height: 600px"
  src="http://jsfiddle.net/OnSIP/vW7Lw/embedded/js,html,css,result/">
</iframe>

