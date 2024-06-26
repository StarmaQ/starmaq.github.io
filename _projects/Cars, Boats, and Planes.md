---
name: Cars, Boats, and Planes!
tools: [Roblox]
image: https://raw.githubusercontent.com/StarmaQ/starmaq.github.io/main/assets/fezfzef.png
description: Advanced rigs for all types of vehicles.
---

I feel like this is one of those common creations a developer wishes to create: primarily a car rig, and then all sorts of fun things, such as a place. Yet I don't think it's that easy to create something beautiful and that works well for each of them, with varying difficulties. Throughought my adventure, it took a lot of fiddling, tweaking with Roblox's physics, trying out a crap ton of different ideas, and searching deep online, and in Discords, for other peoples' help and prior experience on the subject. To be honest, resources were scarce. I relied on a couple of toolbox models, and some youtube videos, specifically for the car rig, before I figured out something nice (quite honestly, by mere luck and the power of trial and error).

**All models and VFX displayed are made respectively by @Viewported and @LuaScripts!** I did the model rigging, and VFX scripting additionally.

## Cars

Funnily enough, the simplest and most basic one was the hardest one to get right! Vehicles were tedious, and harder than all the other types of vehicles I made, for the sole reason of the way they move. 
They weren't simply moved by some central BodyPosition that controlled everything, they relied on the wheels and their friction with the ground, as they rolled using the CylindricalConstraint (which should be your go-to!). This meant (ackermann) steering, suspension, and most horrifyingly collision, were not trivial problems. 
You had to get the PhysicalProperties right, the SpringConstraint for the suspensions correctly tuned, and finding the best configurations for your central mass part, that consituted the mass of the car as everything else was massless. 

The end product was this:
<iframe width="1280" height="546" src="https://www.youtube.com/embed/9A589nvKaOU" title="car1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Look at the smooth suspension and smooth collision! There was even a drift hotkey, with a cool effect, although the drifting is kind of busted. It doens't quite drift, it'll only start steering faster. I gave up on drifting, I just couldn't do it!

I love the way I structured the way cars (and all the vehicles consequently) worked. A `CarController` (I was using Knit), that created a `CarClass`, which initialized the virtual side of every physical car, and updated its physics configurations by exposing a `CarClass:Update(dt)`, that was used by a `CarHandler`, which took care of all the physical car objects, their player interaction, kept track of their destruction, and took care of any side things that weren't included in the `:Update(dt)` method (some VFX for example, especially for planes). I didn't use VehicleSeat, and wanted to write the `SteerFloat` logic myself (and painfully the camera code), and updated the `CylindricalConstraints`'s rotation accordingly with some ackermann steering.

I also learned a lot of things about Roblox physics during my playtime with this. I learned to never set any sort of MaxForce/MaxTorque property to `Vector3.new(math.huge, math.huge, math.huge)`, and instead set it to some reasonable value in the thousands, after some trial and error, else it'll cause all sort of wobbly collision issues. The significance of the center of mass of your assembly, the importance of friction...

It even works really well with bigger vehicles! After some different configurations for the mass part.

<iframe width="1280" height="548" src="https://www.youtube.com/embed/E2feiQSL64c" title="car2" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
I won't yap as much for the next vehicles, since almost the same applies.

## Planes
Anything but the cars was easy! All I needed was a BodyPosition, and a BodyGyro, and just do some acceleration/steering logic on my `:Update(dt)` method. Planes behaved differently, as you didn't need to continuously press forward to move, you just accelerated and decelerated. Additional raycasting was done to prevent stuff like sinking into the ground, it also lands very well but it's not shown in the video. It also nudged along its yaw and roll as it steered and it went up.

<iframe width="1280" height="676" src="https://www.youtube.com/embed/FSgI8nZZjsY" title="plane" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

What was exceptional about this was the very cool effects @LuaScripts made for the plane, and the way I beautifuly programmed them to interact with the plane! There was an effect around the nose of the plane when you were moving at max speed. I made the front wheel, and wing thingies move accordingly to your actions. Flying it was generally fun!

## Helicopter

These ones were almost identical to the planes, with the same setup, along with going up and down. 

<iframe width="1280" height="576" src="https://www.youtube.com/embed/HhR7J-zGUvs" title="helicopter1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

The hard part about this was the rope! I didn't end up finishing that part, it was tedious. I tried a handful of solutions:
* A bare rope constraint, nope, the player flew all over the place, this is after making him massless
* A rope constraint, with a rod constraint, which I discovered were useful, but this bugged out and didn't work well, the length limiting part of this constraint annoyed me while I only needed its angle limitation
* Best solution I got was a body position that I manually set the position of to be offsetted from the helicopter's bottom.

## Boat

Boats were fun! They're unusual vehicles that you don't see often. I had fun programming their buouncy, and their steering is fun. I had to elevate the boat depending on sea level, and how much of their body should be sumberged into water.

<iframe width="1280" height="548" src="https://www.youtube.com/embed/O5dr3kzc30w" title="boat" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

What was fun about this was the little tubing behind the boat! It interacted beautifully with the boat, steered accordingly to the main ship's movement, and stopped very well when it did. I had fun using the rod constraint for this one.

The boat also interacted wonderfully with collision, and moved realisticly when on ground.
