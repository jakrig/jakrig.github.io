---
layout: post
title: Connecting controls to a softMod
description: Connecting controls to a softMod to create palpation. 
img: /icons/palpation/icon.png
date: 2018-07-08 19:18:36
category: rigging, tools
tag: [maya, rigging, tools]
---
How to connect controls to a softMod to create a palpation setup.
<p align="center"><iframe src="https://player.vimeo.com/video/351012708?color=ff9933&title=0&byline=0&portrait=0" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></p>

<h3>Method</h3> 
<p align="center"><img src="/icons/palpation/palpation_outliner.png"/></p>
<p class="justify"> 
This setup is really easy to set up and helped me out a lot. Once you have your control hierarchy set up, 
multiply the local matrix of your pressure offset group with the local matrix of your pressure control using a multMatrix node.
The sum of this multiplication needs to be plugged into the .softModXforms.weightedMatrix attribute of your softMod. 
Then plug the world matrix of your palpation control into .softModXforms.preMatrix, and the world inverse matrix into .softModXforms.postMatrix.
I used a decomposed world matrix of my palpation control to control my falloff center, but there are other ways to do so.
Finally create a falloff attribute on the pressure control and plug it into the falloff radius.
</p>
<p align="center"><img src="/icons/palpation/palpation_node_network.png"/></p>

<h3>Other setups</h3>
<p class="justify"> 
You can also create a similar setup using lattices. In this case just parent your lattice to your pressure control 
and the lattice base to your palpation control. The falloff attribute needs to be plugged into the .outSideFalloffDist 
attribute of your lattice ffd.
</p>
<p align="center"><img src="/icons/palpation/palpation_lattice_outliner.png"/></p>


