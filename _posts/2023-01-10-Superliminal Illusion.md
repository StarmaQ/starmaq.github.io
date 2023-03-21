---
title: Superliminal Illusion
date: 2023-01-10
categories: [Creations]
tags: [Roblox]     # TAG names should always be lowercase
---

As part of the 2023 VectorThree game jam, whose theme was "looks are deceiving", I decided to implement many illusion mechanics from many games, namely Superliminal, one of the amazing illusions that I wanted to replicate was this one, a "trompe-d'œil":

<iframe width="1237" height="696" src="https://www.youtube.com/embed/VznBnbCNVpc" title="illusion1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

I basically used EgoMoose's epic portal module, by placing the object behind the three walls, seeing their image from the other side, disabling the portals and hardcoding their cameras' position. That way, it'll align only if you're at a certain position, I also save and hardcode that CFrame, and do some checks off of it to see if the item can be collected. Cool!

