# Houdini_Camera_Geo_Culler

A node to help reduce the amount of unnessescary polygons and points in your scene by deleting the polys and points that are not visible to the camera, video game engine style!

Video overview: https://vimeo.com/457405615

## Installation

To install this otl all you have to do is download it and place it in one of these folders:

Windows: C:\Program Files\Side Effects Software\Houdini xx.x.xxx\houdini\otls

Mac: /Applications/Houdini/Houdinixx.x.xxx/houdini/otls

Linux: /opt/hfsxx.x.xxx/houdini/otls

After restarting houdini the node should be avalible to place down in SOPs.
It can be found either by right clicking and hovering over the "digital assets" category or by searching for "Camera Culler".

## Usage

### Frustum Culling Mode

After hooking it up to the geometry stream you want to cull simply select what camera is the render camera in the "Camera" variable and start tweaking the settings to your liking!

By default the culler only uses the camera frustum to cull the geometry, but there are other modes explained further down that can also be turned on to optimize the poly count even further!

The ***ZNear*** and ***ZFar*** parameters control how close to the camera and how far away from the camera the culling will take place. If a point is closer than the ***ZNear*** value or further away than the ***ZFar*** value it will be removed.

The ***Window X*** and ***Window Y*** values control how much of the view of the camera should be kept. The default values of -0.1 and 1.1 give some padding around the edges of the frame so that geometry does not noticably dissapear while it is still in frame. If you can see objects dissapearing at the edge of frame, try making the lowering the -0.1 value and increasing the 1.1 value.

The ***Uniform sampling divs*** control the volume made from the camera frustum's resolution, the higher the value the more accurate the volume, however it will increate calculation time. Only increase if you see very noticable stepping in your geometry after culling.

### Dot Culling Mode

When enabled, this mode will compute point normals and compare the dot product between the surface normal and the direction from the point to the camera and based on the threshold value will either delete or keep it. A threshold value of -1 means that only the points which surface normal are exactly opposite of the direction from the point to the camera are deleted and if the threshold is set to 1 only the points which surface normal exactly aligns with the direction from the point to the camera are kept.

### Ray Culling Mode

When this is enabled, every single point does a ray operation where it shoots a ray from the point towards the camera and if it "hits" a surface on the way it will be deleted because it means that the point is behind some other piece of geometry and as such it cannot be seen by the camera. The "Cull Padding" value allows you to keep more points near the edges of the deleted points from the ray culling. This is to help prevent the edge being noticable from the camera, increasing this value will keep more points and make sure that the culling is not visible to the camera, a smaller value will cull more points but could become visible depending on your geometry's resolution and the speed of your camera animation.