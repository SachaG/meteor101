<h1 class="title1">Introduction to Meteor</h1>
#### @SachaGreif



# So What's Meteor?



## Client-Side JavaScript
<img src="images/meteor-rails.png" class="noborder">



## Server-Side JavaScript
<img src="images/meteor-node.png" class="noborder">



## Full-Stack JavaScript
<img src="images/meteor-client-server.png" class="noborder">
<!-- Meteor is a JavaScript framework with both server-side and client-side components. -->
<!-- # Three Meteor Principles -->



## Single Page App
<img src="images/meteor-data-wire.png" class="noborder">
<!-- The client front-loads all HTML, CSS, and JavaScript code necessary to the app when you first connect, then after that only receives data.  -->



## Database Everywhere
<img src="images/meteor-database-everywhere.png" class="noborder">
<!-- Meteor replicates a subset of the database in the browser's memory for easy access. -->



## Reactivity
<img src="images/meteor-reactivity.png" class="noborder">
<!-- Any modification to the server-side database is reflected in real-time on the client. -->
<!-- # A Few More Cool Things… -->
<!-- ## Everything Included -->
<!-- Meteor automatically loads any HTML, CSS, or JavaScript file included in a repository. -->
<!-- ## Hot Code Reload -->
<!-- Any change to an app's source files automatically triggers a browser refresh. -->
Note:
Meteor takes existing technologies and makes them work together seamlessly, on both client and server. 



# What We're Building



<p><img src="images/dribbble-m.png" class="noborder big"></p>
Note:
The Dribbble homepage: the 12 most popular “shots”.



## Grid + Dribbble =



<!-- <p><img src="images/gribbble.png" class="noborder bigger"></p> -->
<p><img src="images/gribbble-diagram.png" class="noborder"></p>
Note: that's the end of the section with pretty images, it's all code from now on.
<!-- <a href="javascript:void(0)" class="commit-link" data-value="master">Show Me!</a> -->




# Setting Up



## Installing Meteor
```bash
curl https://install.meteor.com | /bin/sh
```



## Creating the App
Create the app.
```bash
meteor create gribbble
```
Open the app's directory.
```bash
cd gribbble
```



## Running the App
```bash
meteor
```
<a href="javascript:void(0)" class="commit-link" data-value="c1-1">Run</a>



## Removing Placeholder Files
```bash
gribbble.css 
gribbble.html 
gribbble.js
```



## Directory Structure
```bash
/client
/server
/collections
```



## The Stylesheet
Copy-paste the stylesheet into `client/style.css`.
<a href="javascript:void(0)" class="commit-link" data-value="c1-2">Run</a>
<!-- ## Recap -->
<!-- We now have a working Meteor app. Let's make it actually do something! -->



### Collecting Data
<p><img src="images/data-diagram.png" class="noborder"></p>



## The Collection
```js
Snapshots = new Meteor.Collection('snapshots');
```
<div class="file">`/collections/snapshots.js` (both)</div>
<div class="highlight" data-coordinates="[null, 'top:111px; left: 56px; height: 40px; width: 134px;', 'top:111px; left: 226px; height: 40px; width: 314px;', 'top:111px; left: 545px; height: 40px; width: 150px;']"></div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c1-3">Run</a> -->
<!-- <p class="test">`Snapshots.find().fetch()`</p> -->
<!-- ## The Dribbble API -->
<!-- <div class="smaller">`http://api.dribbble.com/shots/popular?per_page=12`</div> -->



## The HTTP Package
```bash
meteor add http
```



## Querying the Dribbble API
```js
var url="http://api.dribbble.com/shots/popular?per_page=12";

var queryAPI = function () {
  HTTP.get(url, function(error, result){
    if(result){
      result.timestamp = new Date().getTime();
      Snapshots.insert(result);
    }
  });
}
```
<div class="file">`/server/api.js` (server)</div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c1-4">Run</a> -->
<div class="highlight" data-coordinates="[null, 'top:113px; left: 54px; height: 37px; width: 806px;', 'top:190px; left: 54px; height: 37px; width: 397px;', 'top:230px; left: 81px; height: 37px; width: 537px;', 'top:314px; left: 137px; height: 37px; width: 575px;', 'top:352px; left: 137px; height: 37px; width: 360px;']"></div>



## Running Every Hour
```js
Meteor.setInterval(queryAPI, 3600000);
```
<div class="file">`/server/api.js` (server)</div>
<div class="highlight" data-coordinates="[null, 'top:112px; left: 54px; height: 37px; width: 267px;', 'top:112px; left: 324px; height: 37px; width: 125px;', 'top:112px; left: 467px; height: 37px; width: 107px;']"></div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c1-5">Run</a> -->
<!-- ## Recap -->
<!-- We're now collecting the 12 most popular Dribbble shots every hour. Next step: displaying them.  -->



### Displaying Data
<img src="images/ui-diagram.png" class="noborder">



<img src="images/templates.png" class="noborder big">



<img src="images/main.png" class="noborder big">



## The `main` Template
```html
<head>
  <title>Gribbble</title>
</head>

<body>
  <h1>Gribbble</h1>
  {{> grid}}
</body>
```
<div class="file">`/client/main.html` (client)</div>
<div class="highlight" data-coordinates="[null, 'top:111px; left: 56px; height: 119px; width: 370px;', 'top:269px; left: 56px; height: 164px; width: 282px;', 'top:351px; left: 86px; height: 37px; width: 149px;']"></div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c2-1">Run</a> -->



<img src="images/grid.png" class="noborder big">



## A Snapshot
<img src="images/dribbble-api.png" class="noborder ">



## The `grid` Template
```html
<template name="grid">
  <div class="grid">
    {{#each snapshots}}
      <div class="grid-snapshot grid-col">
        {{timestamp}}
      </div>
    {{/each}}
  </div>
</template>
```
<div class="file">`/client/grid.html` (client)</div>
<div class="highlight" data-coordinates="[null, 'top:112px; left: 56px; height: 37px; width: 318px;', 'top:190px; left: 115px; height: 37px; width: 267px;', 'top:271px; left: 174px; height: 37px; width: 187px;']"></div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c2-2">Run</a> -->



## The Data Context
<img src="images/data-context-1.png" class="noborder ">
<!-- .slide: data-transition="none" -->



## The Data Context
<img src="images/data-context-2.png" class="noborder ">
<!-- .slide: data-transition="none" -->



## The `find()` Syntax
```js
collection.find(selector, options);
```
- `selector`: what to find.
- `options`: sort, limit, etc.



## The `snapshots` Helper
```js
Template.grid.helpers({
  snapshots: function () {
    return Snapshots.find(
      {}, 
      {
        sort: {timestamp: -1}, 
        limit: 12
      }
    );
  }
});
```
<div class="file">`/client/grid.js` (client)</div>
<a href="javascript:void(0)" class="commit-link" data-value="c2-3">Run</a>
<div class="highlight" data-coordinates="[null, 'top:111px; left: 54px; height: 40px; width: 331px;', 'top:150px; left: 81px; height: 40px; width: 344px;', 'top:189px; left: 111px; height: 40px; width: 312px;', 'top:229px; left: 142px; height: 40px; width: 40px;', 'top:270px; left: 142px; height: 152px; width: 340px;']"></div>



## Looping Over `data.shots`
```html
<template name="grid">
  <div class="grid">
    {{#each snapshots}}
      <div class="grid-snapshot grid-col">
        {{#each data.shots}}
          <p>{{title}}</p>
        {{/each}}
      </div>
    {{/each}}
  </div>
</template>
```
<div class="file">`/client/grid.html` (client)</div>
<a href="javascript:void(0)" class="commit-link" data-value="c2-4">Run</a>
<div class="highlight" data-coordinates="[null, 'top:189px; left: 115px; height: 40px; width: 271px;', 'top:270px; left: 175px; height: 40px; width: 283px;', 'top:310px; left: 199px; height: 40px; width: 233px;']"></div>



<img src="images/shot.png" class="noborder big">



## The `shot` Template
```js
<template name="shot">
  <a href="{{url}}" target="_blank" class="grid-shot">
    <img src="{{image_teaser_url}}" class="grid-image"/>
  </a>
</template>
```
<div class="file">`/client/shot.html` (client)</div>
<div class="highlight" data-coordinates="[null, 'top:111px; left: 66px; height: 40px; width: 314px;', 'top:151px; left: 222px; height: 40px; width: 98px;', 'top:189px; left: 263px; height: 40px; width: 275px;']"></div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c2-5">Run</a> -->



## Including the `shot` Template
```html
<template name="grid">
  <div class="grid">
    {{#each snapshots}}
      <div class="grid-snapshot grid-col">
        {{#each data.shots}}
          {{>shot}}
        {{/each}}
      </div>
    {{/each}}
  </div>
</template>
```
<div class="file">`/client/grid.html` (client)</div>
<div class="highlight" data-coordinates="[null, 'top:310px; left: 200px; height: 40px; width: 132px;']"></div>
<a href="javascript:void(0)" class="commit-link" data-value="c2-6">Run</a>
<!-- ## Recap -->
<!-- We're storing and displaying data. We now need to control the flow of data from server to client.  -->



### Controlling Data
<p><img src="images/flow-diagram.png" class="noborder"></p>



## Removing `autopublish`
```bash
meteor remove autopublish
```
<!-- <p class="test">`Snapshots.find().fetch()`</p> -->



## Removing `insecure`
```bash
meteor remove insecure
```
<a href="javascript:void(0)" class="commit-link" data-value="c3-1">Run</a>



### Your App
<p><img src="images/bookstore.jpg" class="border shutdown"></p>



### Publications
<p><img src="images/publication.jpg" class="border"></p>



### Subscriptions
<img src="images/subscription.jpg" class="border">



## Addding a Publication
```js
Meteor.publish('snapshots', function(clientLimit) {
  return Snapshots.find(
    {}, 
    {
      sort: {timestamp: -1}, 
      limit: clientLimit
    }
  );
});
```
<div class="file">`/server/publications.js` (server)</div>
<div class="highlight" data-coordinates="[null, 'top:110px; left: 53px; height: 40px; width: 208px;', 'top:110px; left: 272px; height: 40px; width: 158px;', 'top:110px; left: 575px; height: 40px; width: 178px;', 'top:150px; left: 83px; height: 273px; width: 373px;', 'top:309px; left: 240px; height: 40px; width: 169px;']"></div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c3-2">Run</a> -->



## Subscribing to the Publication
```js
Meteor.subscribe('snapshots', 12);
```
<div class="file">`/client/main.js` (client)</div>
<a href="javascript:void(0)" class="commit-link" data-value="c3-3">Run</a>
<!-- <p class="test">`Snapshots.find().fetch()`</p> -->
<!-- ## Recap -->
<!-- We built a simple web app in less than 100 lines of code, and saw an overview of the main Meteor features.  -->



### Extra Credit
<img src="images/gribbble-diagram.png" class="noborder">



## The `zoom` Template
<p><img src="images/zoom.png" class="noborder bigger"></p>



## Hover Events
```js
Template.shot.events({
  'mouseenter': function (event) {
    this.image_teaser_url = this.image_url;
    Session.set('zoomedShot', this);
  },
  'mouseleave': function (event) {
    Session.set('zoomedShot', null);
  }
});
```
<div class="file">`/client/shot.js` (client)</div>
<div class="highlight" data-coordinates="[null, 'top:111px; left: 56px; height: 39px; width: 320px;', 'top:151px; left: 90px; height: 39px; width: 168px;', 'top:190px; left: 113px; height: 39px; width: 551px;', 'top:230px; left: 113px; height: 39px; width: 168px;', 'top:230px; left: 285px; height: 39px; width: 168px;', 'top:230px; left: 475px; height: 39px; width: 67px;', 'top:310px; left: 90px; height: 39px; width: 168px;', 'top:350px; left: 475px; height: 39px; width: 65px;']"></div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c4-1">Run</a> -->
<!-- <p class="test">`Session.get('zoomedShot')`</p> -->




## Include the `shot` Template
```html
<template name="zoom">
  <div class="zoom-shot">
    {{> shot zoomedShot}}
  </div>
</template>
```
<div class="file">`/client/zoom.html` (client)</div>
<div class="highlight" data-coordinates="[null, 'top:111px; left: 56px; height: 39px; width: 320px;', 'top:191px; left: 115px; height: 39px; width: 296px;', 'top:191px; left: 242px; height: 39px; width: 146px;']"></div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c4-2">Run</a> -->



## The `zoomedShot` Helper
```js
Template.zoom.helpers({
  zoomedShot: function () {
    return Session.get('zoomedShot');
  }
});
```
<div class="file">`/client/zoom.js` (client)</div>
<div class="highlight" data-coordinates="[null, 'top:111px; left: 56px; height: 39px; width: 320px;', 'top:152px; left: 87px; height: 39px; width: 146px;', 'top:191px; left: 210px; height: 39px; width: 351px;']"></div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c4-3">Run</a> -->



## Include the `zoom` Template
```html
<body>
  <h1>Gribbble</h1>
  {{> grid}}
  {{> zoom}}
</body>
```
<div class="file">`/client/main.html` (client)</div>
<div class="highlight" data-coordinates="[null, 'top:231px; left: 86px; height: 39px; width: 149px;']"></div>
<a href="javascript:void(0)" class="commit-link" data-value="c4-4">Run</a>



## Add a Loading Screen
```html
<body>
  <h1>Gribbble</h1>
  {{> grid}}
  {{> zoom}}
  <div class="loading"></div>
</body>
```
<div class="file">`/client/main.html` (client)</div>
<div class="highlight" data-coordinates="[null, 'top:270px; left: 86px; height: 39px; width: 389px;']"></div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c4-7">Run</a> -->



## Hide the Loading Screen
```js
Deps.autorun(function(){
  Meteor.subscribe('snapshots', 12, 
    function () {
      $('.loading').fadeOut('slow');
    });
});
```
<div class="file">`/client/main.js` (client)</div>
<div class="highlight" data-coordinates="[null, 'top:111px; left: 55px; height: 39px; width: 180px;', 'top:191px; left: 112px; height: 121px; width: 459px;', 'top:231px; left: 136px; height: 39px; width: 429px;']"></div>
<a href="javascript:void(0)" class="commit-link" data-value="c4-8">Run</a>



# Thanks!
<!-- #### Now go build something cool :) -->