---
layout: post
title: "Unity's TryGetComponent Method"
summary: "A brief recap of what I've been doing in Unity lately"
author: hunterpankey
date: '2024-05-11 00:35:00 -0400'
category: ['unity','dev', 'gamedev', 'scripting']
tags: 
thumbnail: /assets/img/posts/unity-trygetcomponent-method/header.png
keywords: unity dev gamedev
usemathjax: false
permalink: /blog/unity-trygetcomponent-method/
classes: wide
---
Starting in Unity 2019.2, GameObjects now have a [TryGetComponent()][trygetcomponent] method that attempts to get a component on the game object and return it (via an out parameter), as well as returning a boolean for whether the operation was successful. One should basically never use the old [GameObject.GetComponent()][getcomponent] method now that TryGetComponent() is available. (I'm sure a good use case could be coerced from some scenario, but for most cases, this new one'll do much better.)

Basically, instead of doing:

{% highlight csharp %}
Rigidbody rb = this.GetComponent<Rigidbody>();

if(rb != null)
{
  // do whatever ole thing with the Rigidbody
}
{% endhighlight %}

This can be kinda cleaned up into the following:

{% highlight csharp %}
if(this.TryGetComponent<Rigidbody>(out Rigidbody rb))
{
  // do whatever you were gonna do with the Rigidbody using the rb variable
}
{% endhighlight %}

They both accomplish basically the same thing, but the second is a little more concise.

Note: There's a similar pattern when raycasting with [Physics.Raycast()][physics-raycast] and similar methods:

{%highlight csharp%}
if(Physics.Raycast(position, direction, out RaycastHit hit, distance, layerMask))
{
  // hit was successful, use hit variable here
}
{%endhighlight%}

[trygetcomponent]: https://docs.unity3d.com/ScriptReference/Component.TryGetComponent.html
[getcomponent]: https://docs.unity3d.com/ScriptReference/Component.GetComponent.html
[physics-raycast]: https://docs.unity3d.com/2019.2/Documentation/ScriptReference/Physics.Raycast.html