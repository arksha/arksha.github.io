---
layout: post
title: Graphic Recap
date: 2016-03-22
categories: Graphics
---
##Real time rendering third Edition book review

###Chap 2. The Graphic rendering pipeline

1.The Architecture:  
Contains Application stage, Geometry stage, Rasterizer stage.

FPS and HZ:
fps: the slowest of the pipeline stages determines the renderding speed, expressed as fps
Hz: use hardware more

The Application stage:   
execute on CPU, fully control. Usually implement in parallel on several process cores, called superscalar construction.  

Usually implement on this stage:  
collision detection and generate response,  
input from other sources and take different actions,  
texture animation,animation via transforms,  
acceleration algorithms.





