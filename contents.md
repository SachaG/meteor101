# Meteor 101

@SachaGreif

Discover Meteor



# How It All Started



<p><img src="images/hackernews.png" class="noborder bigger"></p>
Note:
At the time, there wasn't anything like Hacker News for designers. 



<p><img src="images/meteor.png" class="noborder bigger"></p>
Note:
“Wow, a new, experimental, undocumented web framework! What a great way to save some time!”



<p><img src="images/telescope.png" class="noborder bigger"></p>



<p><img src="images/sidebar.png" class="noborder bigger"></p>



<p><img src="images/discovermeteor.png" class="noborder bigger"></p>
<!-- <p><img src="images/discovermeteorcontent.png" class="noborder bigger"></p> -->



# So What's Meteor?



## Client-Side JavaScript
<p><img src="images/meteor-rails.png" class="noborder"></p>



## Server-Side JavaScript
<p><img src="images/meteor-node.png" class="noborder"></p>



## Full-Stack JavaScript
<p><img src="images/meteor-client-server.png" class="noborder"></p>
<!-- Meteor is a JavaScript framework with both server-side and client-side components. -->



# Three Meteor Principles



## Data on the Wire
<p><img src="images/meteor-data-wire.png" class="noborder"></p>
<!-- The client front-loads all HTML, CSS, and JavaScript code necessary to the app when you first connect, then after that only receives data.  -->



## Database Everywhere
<p><img src="images/meteor-database-everywhere.png" class="noborder"></p>
<!-- Meteor replicates a subset of the database in the browser's memory for easy access. -->



## Reactivity
<p><img src="images/meteor-reactivity.png" class="noborder"></p>
<!-- Any modification to the server-side database is reflected in real-time on the client. -->



# A Few More Cool Things…



## Everything Included
<!-- Meteor automatically loads any HTML, CSS, or JavaScript file included in a repository. -->



## Hot Code Reload
<!-- Any change to an app's source files automatically triggers a browser refresh. -->
Note:
Meteor takes existing technologies and makes them work together seamlessly, on both client and server. 



# What We're Building



<p><img src="images/dribbble-m.png" class="noborder bigger"></p>
Note:
The Dribbble homepage: the 12 most popular “shots”.



## Grid + Dribbble = Gribbble
Showing the Dribbble homepage over the past 12 hours. 



<p><img src="images/daybbble-diagram-2.png" class="noborder bigger"></p>
A grid of the homepage over time.



<p><img src="images/gribbble-diagram.png" class="noborder"></p>
Note: that's the end of the section with pretty images, it's all code from now on.



## The App
<!-- <a href="javascript:void(0)" class="commit-link" data-value="master">Show Me!</a> -->




# Step 1: Setting Up



## Install Meteor
```bash
curl https://install.meteor.com | /bin/sh
```



## Create an App
Create the app.
```bash
meteor create gribbble
```
Open the app's directory.
```bash
cd gribbble
```



## Run the App
```bash
meteor
```
<a href="javascript:void(0)" class="commit-link" data-value="c1-1">Run</a>



## File Structure
- Remove Meteor's placeholder files.
```bash
rm gribbble.css gribbble.html gribbble.js
```
- Create three directories
```bash
mkdir client
mkdir server
mkdir collections
```
- Copy-paste the stylesheet into `client/style.css`.
<a href="javascript:void(0)" class="commit-link" data-value="c1-2">Run</a>



## Recap
We now have a working Meteor app. Let's make it actually do something!



# Step 2: Collecting Data



<p><img src="images/data-diagram.png" class="noborder"></p>



## Create a Collection
```js
Snapshots = new Meteor.Collection('snapshots');
```
<div class="file">`/collections/snapshots.js` (both)</div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c1-3">Run</a> -->
<!-- <p class="test">`Snapshots.find().fetch()`</p> -->



## The Dribbble API
<div class="smaller">`http://api.dribbble.com/shots/popular?per_page=12`</div>



<p><img src="images/dribbble-api.png" class="noborder "></p>



## Query the Dribbble API
```bash
meteor add http
```

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



## Schedule to Run Every Hour
```js
Meteor.setInterval(function(){
  queryAPI();
}, 3600000);
```
<div class="file">`/server/api.js` (server)</div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c1-5">Run</a> -->



## Recap
We're now collecting the 12 most popular Dribbble shots every hour. Next step: displaying them. 



# Step 3: Displaying Data
<!-- ## Preloading Data
Let's preload data into our database. 
```js
var url = 'https://rawgithub.com/SachaG/9798180/raw/ab3c76d1b13578bc7efe66536f4af6f4486cbfb7/Snapshots.json';
$.getJSON(url, function(data){
  _.each(data, function(snapshot){Snapshots.insert(snapshot)});
});
```
(Copy to JavaScript console and run) -->



<p><img src="images/ui-diagram.png" class="noborder"></p>



## The Main Template
<p><img src="images/main.png" class="noborder bigger"></p>



## The Main Template
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
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c2-1">Run</a> -->



## The Grid
<p><img src="images/grid.png" class="noborder bigger"></p>



## The Grid
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
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c2-2">Run</a> -->



## Fill The Grid With Data
```js
Template.grid.helpers({
  snapshots: function () {
    return Snapshots.find({}, {
      sort: {timestamp: -1}, 
      limit: 12
    });
  }
});
```
<div class="file">`/client/grid.js` (client)</div>
<a href="javascript:void(0)" class="commit-link" data-value="c2-3">Run</a>



## Loop Over The Shots
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



## The Shot
<p><img src="images/shot.png" class="noborder bigger"></p>



## Add Shot Template
```js
<template name="shot">
  <a href="{{url}}" target="_blank" class="grid-shot">
    <img src="{{image_teaser_url}}" class="grid-image"/>
  </a>
</template>
```
<div class="file">`/client/shot.html` (client)</div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c2-5">Run</a> -->



## Include Shot Template
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
<a href="javascript:void(0)" class="commit-link" data-value="c2-6">Run</a>



## Recap
We're storing and displaying data. We now need to control the flow of data from server to client. 



# Step 4: Controlling Data



<p><img src="images/flow-diagram.png" class="noborder"></p>



<p><img src="images/bookstore.jpg" class="border big"></p>



## Remove Packages
```bash
meteor remove autopublish
meteor remove insecure
```
<a href="javascript:void(0)" class="commit-link" data-value="c3-1">Run</a>
<!-- <p class="test">`Snapshots.find().fetch()`</p> -->



<p><img src="images/publication.jpg" class="border big"></p>



## Add a Publication
```js
Meteor.publish('snapshots', function(limit) {
  return Snapshots.find({}, {
    sort: {timestamp: -1}, 
    limit: limit
  });
});
```
<div class="file">`/server/publications.js` (server)</div>
<!-- <a href="javascript:void(0)" class="commit-link" data-value="c3-2">Run</a> -->



<p><img src="images/subscription.jpg" class="border big"></p>



## Subscribe to the Publication
```js
Meteor.subscribe('snapshots', 12);
```
<div class="file">`/client/main.js` (client)</div>
<a href="javascript:void(0)" class="commit-link" data-value="c3-3">Run</a>
<!-- <p class="test">`Snapshots.find().fetch()`</p> -->



## Recap
We built a simple web app in less than 100 lines of code, and saw an overview of the main Meteor features. 



## Learn More
<p><img src="images/meteor_book_illustration.png" class="noborder"></p>
<!-- - [Learn Meteor](https://www.meteor.com/learn-meteor) -->
<!-- - [Discover Meteor](https://www.discovermeteor.com) -->
<!-- - [Evented Mind](https://www.eventedmind.com/) -->
<!-- - [MeteorHacks](http://meteorhacks.com/) -->



# Thanks!
#### Now go build something cool :)