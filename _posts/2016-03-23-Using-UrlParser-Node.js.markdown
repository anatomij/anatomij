---
title:  "Using URL Parser in Node.js?"
date:   2016-03-23 7:50:00
categories: [jekyll]
tags: [javscript]
---



Node.js is not a simple framework and some things and specific methods of it can be really confusing to the untrained eye. For example, creating a server in Node is fairly simple and can be achieved with some simple lines of code below:

``` ruby
var http = require("http");
var handler = require("./request-handler");
var server = http.createServer(handler.handleRequest);
server.listen(port, ip);
```

All that's left now is to handle the request via our handleRequest method below and then serve the assets:

``` ruby
var helpers = require('./http-helpers');
var url = require('url');

exports.handleRequest = function (req, res) {
var action = actions[req.method];
  if(action) {
    action(req, res);
  }
};

var actions = {
  'GET': function(request, response) {
    var pathname = url.parse(request.url).pathname;
      if(pathname === '/') {
        pathname = archive.paths.siteAssets + "/index.html";
      } else {
        pathname = archive.paths.archivedSites + pathname;
      }
      helpers.serveAssets(response, pathname);
  }
};
```

The reason we have to require the helpers above is because our serverAssets function is in another file, therefore we have to call helpers.serverAssets. The actions object is simply for modular purposes and it is evident that in the exports.handleRequest that is how we determine the proper method request that is being asked of our server.

This is where it get's tricky, because to properly server the request we need to determine the correct pathname to use.
This is where the url.parse(request.url).pathname comes in and allows us to grab the pathname and use it for functionality.

In our 'GET' method we use it to get the correct pathname to pass into our helpers.serveAssets that further uses the correct pathname to read to the file system using fs.readFile, but that is for another post.
