---
title: Receive a Call | SIP.js
---

# Receive a Call

* TOC
{:toc}

Let's walk through core API concepts as we tackle some everyday use cases.

## Overview

### Setup


~~~ html
<!DOCTYPE html>
  <head>
    <script src="sip.js"></script> 
  </head>

  <body>
    <video id="remoteVideo"></video>
    <video id="localVideo" muted="muted"></video>  
    <button type="button" id="callButton">Call</button>
    <button type="button" id="endButton">End</button>
	
    <script src="phone.js"></script>
  </body>
</html>

~~~

In order to make and receive calls and messages you must create a new SIP user agent.  You do so by doing this.

~~~ javascript
// phone.js

function () {
  //Creates the anonymous user agent so that you can make calls
  var config = {
  uri: 'bob@example.com',
  ws_servers: ['wss://edge.example.com'],
  register: true
};
  var userAgent = new SIP.UA(config);

  var answerButton = document.getElementById('answerButton');
  var endButton = document.getElementById('endButton');

  userAgent.start();

  userAgent.on('start', newSessionHandler);
}


function newSessionHandler()
{
  //here you determine whether the call has video and audio
  userAgent.on('connected', function () {
    var options = {
      mediaConstraints: {
        audio: true,
        video: true
      } 
    }
    //makes the call
    var session = userAgent.invite('sip:bob@example.com', options);
    session.on('accepted', onAccept)
  })
}

function onAccept () {
  //gets the video elements
  var remoteVideo = document.getElementById('remoteVideo');
  var localVideo = document.getElementById('localVideo');

  //attached the received video stream to the Video Elements
  remoteVideo.srcObject= session.getRemoteStreams()[0];
  localVideo.srcObject= session.getLocalStreams()[0];

  //plays the Video Elements
  remoteVideo.play();
  localVideo.play();
}

$('#answerButton').addEventListener('click',function () {
  session.answer();
});

$('#endButton').addEventListener('click',function () {
  session.terminate();
});
~~~