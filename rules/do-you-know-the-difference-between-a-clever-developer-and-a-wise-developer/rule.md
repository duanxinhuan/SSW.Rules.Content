---
type: rule
title: Do you know the difference between a Clever Developer and a Wise Developer?
uri: do-you-know-the-difference-between-a-clever-developer-and-a-wise-developer
authors: []
created: 2022-09-21T04:20:52.902Z
guid: dcd52f5f-7ca9-4826-b5f9-215b85d6db1e
---
> Developers are like a fine wine, they get better with age. 

W﻿hen we first start out as a developer, we want to show what we can do by creating complex solutions.As we gain experience, we learn to show our worth by creating simple solutions.

<!--endintro-->

Take this piece of code as an example:

```
<span className="text-xl">{targetedDays === 0 ? "Today" : targetedDays === -1 ? "Yesterday" : targetedDays === 1 ? "Tomorrow" : moment().add(targetedDays, 'days').format("dddd D MMMM YYYY")}</span>
```

One liner! Nailed it🥳 Pity the next developer who has to decipher what is going on!

Now lets take the following code example:

```
	if (targetedDays === -1) {
		return "Yesterday";
	} elseif (targetedDays === 0) {
		return "Today";
	} elseif (targetedDays === 1) {
		return "Tomorrow";
	} else {
		return moment().add(targetedDays, 'days').format("dddd D MMMM YYYY");
	}
}

<span className="text-xl">{ getTargetedDayAsText(targetedDays) }</span>
```

It is nowhere near as terse, but anyone looking at this knows what is going on at a glance.
This is not an overly complicated example but now imagine something like this:

```
var pixelsQuery =
    from y in Enumerable.Range(0, screenHeight)
    let recenterY = -(y - (screenHeight / 2.0)) / (2.0 * screenHeight)
    select from x in Enumerable.Range(0, screenWidth)
           let recenterX = (x - (screenWidth / 2.0)) / (2.0 * screenWidth)
           let point =
               Vector.Norm(Vector.Plus(scene.Camera.Forward,
                                       Vector.Plus(Vector.Times(recenterX, scene.Camera.Right),
                                                   Vector.Times(recenterY, scene.Camera.Up))))
           let ray = new Ray() { Start = scene.Camera.Pos, Dir = point }
           let computeTraceRay = (Func<Func<TraceRayArgs, Color>, Func<TraceRayArgs, Color>>)
            (f => traceRayArgs =>
             (from isect in
                  from thing in traceRayArgs.Scene.Things
                  select thing.Intersect(traceRayArgs.Ray)
              where isect != null
              orderby isect.Dist
              let d = isect.Ray.Dir
              let pos = Vector.Plus(Vector.Times(isect.Dist, isect.Ray.Dir), isect.Ray.Start)
              let normal = isect.Thing.Normal(pos)
              let reflectDir = Vector.Minus(d, Vector.Times(2 * Vector.Dot(normal, d), normal))
              let naturalColors =
                  from light in traceRayArgs.Scene.Lights
                  let ldis = Vector.Minus(light.Pos, pos)
                  let livec = Vector.Norm(ldis)
                  let testRay = new Ray() { Start = pos, Dir = livec }
                  let testIsects = from inter in
                                       from thing in traceRayArgs.Scene.Things
                                       select thing.Intersect(testRay)
                                   where inter != null
                                   orderby inter.Dist
                                   select inter
                  let testIsect = testIsects.FirstOrDefault()
                  let neatIsect = testIsect == null ? 0 : testIsect.Dist
                  let isInShadow = !((neatIsect > Vector.Mag(ldis)) || (neatIsect == 0))
                  where !isInShadow
                  let illum = Vector.Dot(livec, normal)
                  let lcolor = illum > 0 ? Color.Times(illum, light.Color) : Color.Make(0, 0, 0)
                  let specular = Vector.Dot(livec, Vector.Norm(reflectDir))
                  let scolor = specular > 0
                                 ? Color.Times(Math.Pow(specular, isect.Thing.Surface.Roughness),
                                               light.Color)
                                 : Color.Make(0, 0, 0)
                  select Color.Plus(Color.Times(isect.Thing.Surface.Diffuse(pos), lcolor),
                                    Color.Times(isect.Thing.Surface.Specular(pos), scolor))
              let reflectPos = Vector.Plus(pos, Vector.Times(.001, reflectDir))
              let reflectColor = traceRayArgs.Depth >= MaxDepth
                                  ? Color.Make(.5, .5, .5)
                                  : Color.Times(isect.Thing.Surface.Reflect(reflectPos),
                                                f(new TraceRayArgs(new Ray()
                                                {
                                                    Start = reflectPos,
                                                    Dir = reflectDir
                                                },
                                                                   traceRayArgs.Scene,
                                                                   traceRayArgs.Depth + 1)))
              select naturalColors.Aggregate(reflectColor,
                                             (color, natColor) => Color.Plus(color, natColor))
             ).DefaultIfEmpty(Color.Background).First())
           let traceRay = Y(computeTraceRay)
           select new { X = x, Y = y, Color = traceRay(new TraceRayArgs(ray, scene, 0)) };

foreach (var row in pixelsQuery)
    foreach (var pixel in row)
        setPixel(pixel.X, pixel.Y, pixel.Color.ToDrawingColor());
```

Are you going to put your hand up to try and debug why this isn’t returning the expected value?
This is an example of an inexperienced developer creating a complex solution to a simple problem. Some others really good ways senior developers show their wisdom are

* Avoid problems rather than coding around them 
*  Understanding the whole before they start the solution