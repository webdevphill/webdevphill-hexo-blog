---
title: Posting Images to Hexo with Fancybox
date: 2020-10-25 
tags:
- images
- hexo
- blogging
- fancybox
comments: true
categories: Hexo
---

[Landscape](https://awesomeopensource.com/project/hexojs/hexo-theme-landscape?mode=...) theme uses [Fancybox](https://github.com/fancyapps/fancyBox) to display  photos. Either Markdown syntax or fancybox tag plugin can be used to add photos.

### Markdown Tag
The format for displaying images:
``` markdown
# MarkDown
![image caption](<path to image> <image title>)
![Meow Cat](/assets/images/2020-10-25/kittycat1.jpg "Image of Cat")

# Fancybox Tag
{% fancybox img_url [img_caption] [img_title] %}
{% fancybox /assets/images/2020-10-25/kittycat1.jpg "Meow Cat" "Image of Cat" %}

```

However, this is not the standard Fancybox tag syntax, this is a modified version to have different caption text to image title/alt text.
The fancybox.js script was updated as per below...

``` javascript
// themes/landscape/scripts/fancybox.js

hexo.extend.tag.register('fancybox', function(args){
  var original = args.shift();
  var caption = args.shift();
  var title = args.join(' ');

  return '<a class="fancybox" href="' + original + '" title="' + title + '">' +
    '<img src="' + original + '" alt="' + title + '">' + '</a>' +
    (caption ? '<span class="caption">' + caption + '</span>' : '');
});
```

The resulting markdown image should look like so:

![Meow Cat](/assets/images/2020-10-25/kittycat1.jpg "Image of Cat")

The resulting Fancybox tag should look like this:

{% fancybox /assets/images/2020-10-25/kittycat1.jpg "Meow Cat" "Image of Cat" %}
`
