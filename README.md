# Graphics Libraries Test


### What is this? Why?

Honestly, couldn't decide which one to go with. 

I'd been using freeglut + glew for my raycaster game https://github.com/RedactedProfile/BlackLotus and some weird input bugs started showing themselves.
@sifting reminded me that we'd used SDL in the past.  That's what reminded me that GLFW and SFML also exist. 

To be honest, SFML is the one that interested me the most, considering it's a pure C++ implementation, but it also feels the most bloated comapred to it's C lib contemporaries. 

So I created the project to provide a working example for myself of each kit. What I was looking for:

0. Not much configuration fussing
1. Easy window creation
2. Easy opengl context creation
3. I want to control the render loop manually, will it let me and how easy is it
4. I want to control the event loop manually, will it let me and how easy is it
5. I want to use bare opengl 2.0 style calls, will it let me and how easy is it
6. I want to use modern opengl calls, will it let me and how easy is it

## Results

### FreeGLUT + GLEW

Unfortuantely FreeGLUT gets automatic marks docked from me just based on my history with it.   GLUT and GLEW pretty much have to co-exist to do anything useful, and confusingly the lib files are in different locations based on architecture choice: 32bit in the base, 64bit in a subfolder.  Annoying. 

You also have to ship with two 400kb+ DLL files beside your binary.  Also annoying, though not the end of the world. 

GLUT comes with a series of convenience functions.  glutDisplayFunc binds to a function call that gets called every tick.  But then it gets a little bit weird with glutMotionFunc being the mouse controls... kind of.  glutPassiveMotionFunc is actually for mouse controls with no buttons pressed, but glutMotionFunc is for "dragging mouse". Confusing.  Several functions are like this. 

Mercifully once you're done setting configuring your project to use the libs, include the headers, wrangle the dlls, and bind some functions to glut's main abstractions to create a window, context, and main display loop it pretty much gets out of your way from there. You do probably want to use the Input binds, but if I'm going to be completely honest I don't think it's especially great, and prone to not firing properly if the stars aren't aligned just right.    



### SFML

The big problem with SFML is it's a configuration beast. There's a lot of libraries and lot of abstraction here.  However, with this comes a benefit of flexibility. 

The default mode of linking gets you to ship with several DLL's of varying sizes. SFML however ships each library as a static lib as well, allowing you to bake the data into the binary instead. This is a huge plus.  The binary for the GLUT example is 64kb's in debug, but with the sfml example in debug, baring in mind I included sfml window, sfml graphics, sfml system, freetype, winmm, etc all as static libraries, the end resulting binary was surprisingly only 1mb for the full example. I thought that was surprisingly good.  

When it came to use SFML to create a window, it was a one include file and one line of code affair to do so. 
From here you create a nested loop that runs through an abstracted event loop.

The SFML Graphics lib includes several primitive opengl drawing abstractions, such as cirlce or triangle or quad drawing.  But this is completely optional. 
By including the OpenGL.hpp header, epxoses the raw opengl functions ready to be used immediately.  My example makes use of this. 

Overall, quite happy with the result.  I haven't had a chance to try SFML's input library though as of this writing. 

### GLFW

The defacto standard for C based opengl apps. This is the library that Khronos uses to build their sample code. 

### SDL2

Simple DirectMedia Layer is a minimal C based abstraction library that handles the heavy lifting of creating a window, binding an opengl context, and dealing with a wide array of input methods in a minimal way.  Form there it gets out of your way. 

SDL has a different advantage that the other libraries don't have, is how easy it is to get setup with D3D if you're into that. 