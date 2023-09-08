


---

# Camera matrices in openGL

Also applies for other drawing engines

A short article on how to write the camera equations.


Our geometry is defined to be a cube. You can start with a triangle.

Define it with homogenic coordinates: (x,y,z,1)

Adding the 4th dimension makes things easier.



Let's look at the vertex shader.

```glsl
#version 330 core

layout (location = 0) in vec4 aPos;
layout (location = 1) in vec3 aColor;

out vec3 dynamicColor;

uniform mat4 perspective;
uniform mat4 camera;
uniform mat4 model;

void main()
{
    gl_Position = perspective * camera * model * aPos;
    dynamicColor = aColor;
}
```

If you only pay attention to the `gl_Position` part,
we get `aPos` in, a 4x1 vector that has (x,y,z,1) of each vertex. The vertices are in openGL coordinates (which is...? TODO)

The function returns the coordinate transformed with the correct perspective, camera, model.


TODO: show in pictures what this is in reality.



Ok, most of the value of this post is showing images. I have to draw them very carefully.