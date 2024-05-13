---
layout: post
title:  "May 2024: Stuff I've Done in Unity Lately"
summary: "A brief recap of what I've been doing in Unity lately"
author: hunterpankey
date: '2024-05-10 14:30:00 -0400'
category: ['unity','dev', 'gamedev', 'recap']
tags: 
thumbnail: 
keywords: unity dev gamedev traffic tactical strategy rpg water urp synty
usemathjax: false
permalink: /blog/may-2024-unity-recap/
---
I've been playing with various things in Unity lately, what with the asset store sales on over the past few weeks. I definitely feel guilty (to myself, I guess) for spending a bunch of money on cool models and systems and stuff and not doing enough with them. Everything looks so fun and helpful, and when it's all 50-70% off of a more or less reasonable starting price, who can resist?

## Traffic
I'm somewhat obsessed with putting together a modern city/town scene with some actual life to it. The [Synty Polygon City][synty-polygon-city] and [Town][synty-polygon-town] packs are nice, though a bit old at this point, but they're very stationary and static by default. I've done enough of a car traffic system of my own to know what to look for in a traffic system. I've now used a couple of traffic systems and have recently worked with the Gley Games [Mobile Traffic System][gley-games-traffic] one.

It's fairly easy to set up some road segments and some intersections, and I'm happy to see that everything is a spline and not just a simple linear point series. Some other systems don't use splines, and that makes some of the motion very jerky and makes setting up the 90&deg; turns really (really!) tedious.

Setting up traffic signals, on the other hand, _is_ pretty tedious. I've got a prefab with 3 traffic signals (i.e., three units of the three lights for nine lights total), and wiring up the off and on states for all of them for each of the directions for each of the intersections, well, it gets old fast. I would love to have a control component to add to my prefab, which would have the lights wired up and expose an interface to the intersection controller to manipulate. As it is, I've got to drag 18 things from the scene to the component for each direction, or 54-72 things for a three or four legged intersection. Even more if there are turn lanes, so it's not the most fun one can have in game dev.

It doesn't seem to support pedestrian indicators by itself, though, I could rework a traffic indicator or something. In the US and most countries I've been to, at least, there are only two light positions for a crosswalk indicator, and the "yellow light" state is indicated by a blinking red before it goes solid red. So it's not quite a one-to-one mapping.

But the traffic AI itself is a lot more capable than some of the other systems. Cars do a decent job of avoiding each other and taking turns. I think it would be nice if a car were aware of where it was turning and adjusted the look-ahead collider to point toward it, though. They are a little clumsy, but it generally works pretty well!

## Tactical Strategy
I've always been a fan of turn-based tactical strategy games. There must have been an older game that got me started, but the first one I really remember playing was [Jeanne D'Arc on the PSP][jeanne-darc-psp].

I followed along with [Code Monkey's strategy course][codemonkey-strategy-course] last year and had a perfectly fine, functional, but plain, single-scene game. Nothing wrong with that, but when I went to apply everything to a larger scene, I found that the A* implementation performed a bit poorly. As fun as it is to do my own implementation, there are a ton of A* implementations in Unity already, and a ton of systems that build on top of that to provide some tactical strategy game components and functions.

So, enter [Turn Based ToolKit 3][tbtk]. It includes a grid generator, editor tools for marking grid positions as passable, obstacles, walls, etc., unit definition, and some lifecycle niceties. In some basic testing, I find that I can run a map of at least 100x160 squares at good speed. Once again, Synty assets are great, so I'm using the [Polygon Battle Royale][synty-polygon-battle-royale] pack (demo scene only, natch, who's got the time?) to lay out a little scene and get some gameplay going.

Right away, I notice that the grid is done with actual game objects with a mesh renderer to draw the grid. Why does this matter? Well, in a scene with any elevation, for example, the Battle Royale scene with dirt road meshes, the grid rendering is slightly below ground level at points. 

Another way to do this would be to project a decal downward from above so that it hits the surface of whatever mesh constitutes the ground. Further, one can use the layer system to cause decals to only project onto an actual ground mesh. So if you had props or barriers on top of the ground, assuming they were on an excluded layer, the decal wouldn't be visible on their surfaces, but only on the ground layer object below it.

The system does allow me to specify my own prefab for each grid position, so it will (would) be no big deal to create my own grid tile prefab that included the decal component. Will I ever get around to doing that? *shrug*

## Water
How many millions of water rendering systems are there out there? I must have 15 of them from the various asset store purchases, Humble Bundles, lightning sales, etc., so I'm spoiled for choice, here. I played around with the [Crest water system][crest] for a few hours the other night. It's fairly trivial to get a decent looking water plane showing up with some waves, some surface foam, and good looking caustics under the shallow water.

There are also spline components in Crest to set up rivers and other bodies of water in addition to the surrounding sea. The [Polygon Jungle Biome][synty-polygon-biome-jungle] pack has a little waterfall and stream that flows out from the middle of the island to the sea. I had trouble running a single spline from the high waterfall to the pool below and the stream. It was tough to get the spline's control points in the right position to bend it sharply enough to make it realistic, so I ended up doing to separate splines to make it right. One can set a flow rate, width, and wave strength per spline point, so it's possible to make water flow faster in constricted narrow parts of the stream and slower in wider parts, so that's nice. Merging the end of the spline at the sea with the sea height isn't done automatically. One has to manually position it appropriately. No worries.

## Car Creator/Customizer
I had been developing a little isometric car racing game using the [Polygon Street Racer Pack][synty-polygon-street-racer]. I love the individual moveable parts of the cars - doors, windows, steering wheel, etc., and all the customizations like the paint jobs, spoilers, and whatnot. I got as far as running a multi-lap race with (extremely dumb) AI, with a series of races (i.e., race state was restartable/resettable), with multiple, and modular course configurations before losing steam.

I wish I had done a programmatic car configuration system, though. A game like that cries out to allow players to buy little car customizations with their racing winnings. So I started on something like that a few weeks ago. Eventually, I want to have an in-game customization where a player can select their parts, and probably the selection affects the car's stats during racing. But for now, we're looking at editor windows where I can select various scriptable objects representing the car type (Exotic, Sedan, etc.) and selection of customizations (_this_ paint job, _this_ set of fenders, _this_ front bumper, etc.), and it'll instantiate the base prefab and make all the needed modifications. I've only set up the Exotic and Sedan types so far, but it seems modular enough that I'll be able to set up all the other types.

I do find myself going down the rabbit hole of not knowing how much detail/configurability to allow. For example, there are lots of modular parts to make a nitrous boost. Should players be able to build their own custom boost thing from the parts (no, too much effort for the return), should I make a million custom ones for players to choose from (ugh, still so much effort), should I make like four options (much more likely to be worth it)? I feel like making a few options and being able to choose from those at runtime is a fine compromise.

## ORK Framework (More RPG Shenanigans)
I picked up the [ORK Framework][ork] somewhere along the line. There actually are pretty good docs for it, but it's such a huge system that it would take a long time to truly get up to speed. I'm maybe halfway through their [introduction series (30 parts)][ork-3d-tutorial] with most things working fine. After a battle, the system doesn't go back to the world overview, but I'm sure I missed something somewhere. Should be a fun needle to find in the haystack of configuration.

[synty]: https://syntystore.com
[gley-games-traffic]: https://assetstore.unity.com/packages/tools/behavior-ai/mobile-traffic-system-v2-0-277301
[synty-polygon-city]: https://syntystore.com/products/polygon-city-pack
[synty-polygon-town]: https://syntystore.com/products/polygon-town-pack
[tbtk]: https://assetstore.unity.com/packages/tools/game-toolkits/turn-based-toolkit-3-tbtk-3-138465
[codemonkey-strategy-course]: https://www.gamedev.tv/p/unity-turn-based-strategy?coupon_code=CODE-MONKEYS
[synty-polygon-battle-royale]: https://syntystore.com/products/polygon-battle-royale-pack
[jeanne-darc-psp]: https://strategywiki.org/wiki/Jeanne_d%27Arc
[crest]: https://assetstore.unity.com/packages/tools/particles-effects/crest-water-system-urp-ocean-rivers-lakes-141674
[synty-polygon-biome-jungle]: https://syntystore.com/products/polygon-tropical-jungle-nature-biome
[synty-polygon-street-racer]: https://syntystore.com/products/polygon-street-racer
[ork]: https://orkframework.com/
[ork-3d-tutorial]: https://orkframework.com/guide/tutorials/3d-rpg-quickstart/start-3d-rpg-quickstart/