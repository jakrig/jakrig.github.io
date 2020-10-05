---
layout: post
title: PolyCut based fluid simulation
description: Experimenting with polyCuts to find an animatable fluid solution. 
img: /icons/fluid/fluid.png
date: 2018-04-05 18:18:00
category: rigging, tools
tag: [maya, rigging, tools]
---
Experimenting with polyCuts to find an animatable fluid solution.
<p align="center"><iframe src="https://player.vimeo.com/video/351359997?color=ff9933&title=0&byline=0&portrait=0" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></p>

<h3>Method</h3> 
<p align="center"><img src="/icons/fluid/fluid_graph.png"/></p>
<p class="justify"> 
This setup requires at least a root control, fluid control and a fluid mesh. 
The fluid control has attributes for an air gap, air gap blending and a switch between world and local space. 
Volume preservation is also included. 
I initially created this to find a solution to easily animate fluid in a container,
but it has also helped with bone burring, wire insertion and screw fixation amongst other things.
</p>
<p align="center"><img src="/icons/fluid/fluid_outliner.png"/></p>

<h3>Conclusion</h3>
<p align="center"><img src="/icons/fluid/fluid_nodes.png"/></p>
<p class="justify">This Setup isn't ideal, but it's still a quick, simple and solid solution if you don't want to use simulations.
I did use this setup as a base for many other uses, so it was worth looking into it. 
</p>



