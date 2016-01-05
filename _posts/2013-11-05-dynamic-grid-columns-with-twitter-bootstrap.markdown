---
layout: post
title: Dynamic grid columns with Twitter Bootstrap
date: '2013-11-05'
tags:
- bootstrap
- css
- grid
- javascript
---

In one page of our application, some boxes are displayed in two columns. Sometimes, however, one of the boxes is not displayed.

We did not want the box empty space to stay there. Calculating the grids on the server-side was not the way to go either.

So, with some javascript and we have .col-md-auto (works with other sizes too):

[code language="javascript"]  
$(document).on("ready", function() {  
 $.each(['xs', 'sm', 'md', 'lg'], function(idx, gridSize) {  
 $('.col-' + gridSize + '-auto:first').parent().each(function() {  
 //we count the number of childrens with class col-md-6  
 var numberOfCols = $(this).children('.col-' + gridSize + '-auto').length;  
 if (numberOfCols \> 0 && numberOfCols \< 13) {  
 minSpan = Math.floor(12 / numberOfCols);  
 remainder = (12 % numberOfCols);  
 $(this).children('.col-' + gridSize + '-auto').each(function(idx, col) {  
 var width = minSpan;  
 if (remainder \> 0) {  
 width += 1;  
 remainder--;  
 }  
 $(this).addClass('col-' + gridSize + '-' + width);  
 });  
 }  
 });  
 });  
});

[code language="html"]  
\<div class="row"\>  
 \<div class="col-md-auto"\>  
 \<p\>Hi\</p\>  
 \</div\>  
 \<div class="col-md-auto"\>  
 \<p\>Hi\</p\>  
 \</div\>  
 \<div class="col-md-auto"\>  
 \<p\>Hi\</p\>  
 \</div\>  
 \<div class="col-md-auto"\>  
 \<p\>Hi\</p\>  
 \</div\>  
 \<div class="col-md-auto"\>  
 \<p\>Hi\</p\>  
 \</div\>  
\</div\>

Turns into:

[code language="html"]  
\<div class="row"\>  
 \<div class="col-md-auto col-md-3"\>  
 \<p\>Hi\</p\>  
 \</div\>  
 \<div class="col-md-auto col-md-3"\>  
 \<p\>Hi\</p\>  
 \</div\>  
 \<div class="col-md-auto col-md-2"\>  
 \<p\>Hi\</p\>  
 \</div\>  
 \<div class="col-md-auto col-md-2"\>  
 \<p\>Hi\</p\>  
 \</div\>  
 \<div class="col-md-auto col-md-2"\>  
 \<p\>Hi\</p\>  
 \</div\>  
\</div\>

Only works with \< 12 columns :-)

