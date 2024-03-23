---
title: Zeno's Lerp
date: 2023-03-15
categories: [Problem Solving]
tags: [Roblox]     # TAG names should always be lowercase
---

I present to you this rather uninteresting problem, yet very common I found out recently! Suppose you have a moving object, and you want to smoothly transition your camera towards some offsett from its position. The target position is constantly moving, and the problem arises when the target position changes faster than the object moving towards it, or the impossibility of checking whether it reached it, due to the error constant error between the two.

I present to you, the Zeno Lerp!
```lua
local function ZenoLerp(a, b, fraction, deltaTime)
    local f = 1.0 - math.pow(1.0 - fraction, deltaTime)

    return variableA:Lerp(variableB, f)
end

local A = workspace.A
local B = workspace.B

game:GetService("RunService").RenderStepped:Connect(function(dt)
    local a, b = A.CFrame, B.CFrame
    B.CFrame = SmoothLerp(b, a - Vector3.new(8, 0, 0), 1/6, dt)
end)
```

This lerp follows the same idea as Zeno's paradox, you get to a distination by moving $\frac{1}{2}$ of the distance left to travel, except here we generalize it by moving $\frac{1}{x}$ the way each time, (e.g. $\frac{1}{6}$). No need to explain the black magic behind the function, it's a [black box](https://en.wikipedia.org/wiki/Black_box#:~:text=In%20computer%20programming%20and%20software,being%20executed%20is%20not%20examined.). I guess it ends up catching up to the changing target position because the error between the two gets super small eventually?

Here is the behavior, it'll magically catch up to the target position:

<iframe width="1280" height="252" src="https://www.youtube.com/embed/KYHEXa8nRmQ" title="zeno lerp" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

But there is an even simpler solution: just lerp a first number value from 0 to 1, then lerp the CFrame and pass that number value each tile. The value is guaranteed to get to 1, so the CFrame will eventually be equal to the target CFrame, which is what we're looking for.

```lua
local function lerp(a,b,t) return a * (1-t) + b * t end

local elapsedTime, totalTime = 0, 4

local A = workspace.A
local B = workspace.B

game:GetService("RunService").RenderStepped:Connect(function(dt)
    elapsedTime += dt
    local alpha = math.min(elapsedTime/totalTime, 1)

    B.CFrame = B.CFrame:lerp(A.CFrame - Vector3.new(8, 0, 0), alpha)
end)
```

And also works like a charm, and interestingly different:

<iframe width="1280" height="318" src="https://www.youtube.com/embed/NB7UEOkSUpo" title="simple lerp" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Right so to the interesting part, I stumbled upon this problem when I wanted to add an option to my cars that makes the camera transition to their front or back depending on direction. I had to lerp the camera as the car was moving. Now the cool zeno lerp function did this:

<iframe width="1280" height="564" src="https://www.youtube.com/embed/u-GrSDLA64A" title="zeno lerp car" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

You can see it didn't do a good job, it didn't catch up to the moving car, it just jittered weirdly and I had to just roughly set the position to the target position after some threshold. 

The second simpler solution worked fine though.

<iframe width="1280" height="570" src="https://www.youtube.com/embed/krtWBitLdZs" title="simple lerp car" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

So uh what did we learn? If a solution is good on paper it might not always work, always look for something simpler.

Why did I name the post after the failed solution? Because it had a good name.