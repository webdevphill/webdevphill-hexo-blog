---
title: Netlify Redirects
comments: true
date: 2020-11-16 20:14:55
tags:
- SPA
- Angular
- Routing
- Netlify
- Redirect
categories: Netlify
---

### Single Page Application Routing

If you enter a specific URL route for your SPA, by default Netlify won't know what to do with it and your page won't load.

![](/assets/images/2020-11-16/PageNotFound.png "Page Not Found")

To fix ths you will need to add a _redirects file to your source directory.

``` json
src
 - _redirects
```

Update your angular.json file so that it gets picked up as an asset.

``` json
...
    "assets": [
        "src/favicon.ico",
        "src/assets",
        "src/_redirects"
    ],
...
```

For your SPA you will need to redirect verything back to index.html.
Add the folling to the _redirects file.

``` json
/* /index.html 200
```

Deploy your app and refresh your page...done.