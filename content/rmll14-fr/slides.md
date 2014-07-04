## Node.js

- Javascript coté serveur
- Asynchrone
- Moteur v8 de Chrome

%%%

**
Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications.
Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.
**

%%%

# Let's hack

- Node!
- Un drone
- De la chance (ou des $$$)

%%%

## Décollage!

```
bash-3.2$ npm install ar-drone
```

```
var arDrone = require('ar-drone');
var client  = arDrone.createClient();

client.takeoff();

client
  .after(5000, function() {
    this.clockwise(0.5);
  })
  .after(3000, function() {
    this.stop();
    this.land();
  });

```

%%%

# Drone as a Service

%%%

## Express.js FTW!

```
bash-3.2$ npm install express
```

```
var arDrone = require('ar-drone');
var client  = arDrone.createClient();

var express = require('express');
var app = express();

app.get('/drone/takeoff', function(req, res) {
  drone.takeOff();
});

app.get('/drone/land', function(req, res) {
  drone.land();
});
```

%%%

# Capteurs!

%%%

## Camera

```
drone.createPngStream().on('data', function(frame) {
  // ...
});
```

%%%

## Video

```
// Server side
var dronestream = require('dronestream');
// ...
var http = require('http');
var server = http.createServer(app);
dronestream.listen(server);
```

``` 
<!-- Client side -->
var stream = new NodecopterStream(document.getElementById('stream'));
```

%%%

## Données de navigation

```
var client = require('ar-drone').createClient();
client.config('general:navdata_demo', 'TRUE');

client.on('navdata', function(data) {
  console.log('NAVDATA :: ', data);
});
```

%%%

## Websockets?

```
      -------                 ------                  -----
    | browser | <---------> | server | <----------> | drone |
      -------                 ------                  -----
```

%%%

## Socket.io rocks!

**Socket.IO enables real-time bidirectional event-based communication.
It works on every platform, browser or device, focusing equally on reliability and speed.**

%%%

```
bash-3.2$ npm install socket.io
```

```
var sio = io.listen(server);
sio.set('destroy upgrade', false);
sio.sockets.on('connection', function (socket) {
  console.log('New Socket.IO connection');

  client.on('navdata', function(data) {
    console.log('NAVDATA :: ', data);
    socket.emit('navdata', data);
  });

  socket.on('disconnect', function () {
    console.log('Socket.IO is now disconnected');
  });
});
```

%%%

<img class="stretch" src="./img/epoch.png"/>

%%%

# S'intégrer avec le monde extérieur...

%%%

# Twitter!

%%%

### Faire décoller le drone grace à un tweet

```
bash-3.2$ npm install twitt
```

```
var Twitter = require('twit');
var twiter = new Twitter({/*API KEYS*/});

var stream = twitter.stream('/statuses/filter', {track: 'yolo'});

stream.on('tweet', function(tweet) {
  console.log('Got a tweet', tweet);
  if (isValid(tweet)) {
    drone.takeOff();
  }
});
```

%%%

<img class="stretch" src="./img/takeofftweet.png"/>

%%%

# La partie la plus risquée du talk
## (Surtout pour le premier rang)

%%%

```
bash-3.2$ npm install ardrone-autonomy
```

```
mission.takeoff()
  .zero()
  .altitude(1)
  .hover(10000)
  .forward(1)
  .hover(10000)
  .taskSync(function() {
    console.log('Beer delivered!');
  })
  .cw(180)
  .go({x:0, y:0})
  .ccw(180)
  .hover(1000)
  .land();
```

%%%

# Cheese!!!

%%%

# WTF?

- [Livrer de la drogue en prison](http://www.lefigaro.fr/international/2014/06/26/01003-20140626ARTFIG00155-un-drone-transportant-de-la-drogue-s-ecrase-dans-une-prison-de-dublin.php)

%%%

# Merci!

- [@tmorsellino](http://twitter.com/tmorsellino)
- [https://github.com/linagora/openpaas-rse](https://github.com/linagora/openpaas-rse)
- [Slides](http://chamerling.github.io/slides/content/rmll14-fr/)
- [Linagora](http://linagora.com)
- [Linagora Labs](http://research.linagora.com)

%%%

- [@AwesomePaaS](https://twitter.com/AwesomePaaS)
- [@gSafe_Project](https://twitter.com/gsafe_project)
