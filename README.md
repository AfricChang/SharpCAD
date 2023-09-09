# SharpCAD
SharpCAD is a free software that creates 3D objects using scripts, similar to OpenSCAD. However, it uses C# scripts and currently only supports Windows. Although it is currently only a free software and not open source, it may be open sourced in the future.
Like OpenSCAD, SharpCAD provides two main modeling techniques:

1、Constructive Solid Geometry (CSG): SharpCAD defines 2D objects such as Circle and Square, as well as 3D basic objects like Box and Sphere. However, unlike OpenSCAD, you need to use +, -, and * operators for Boolean union, difference, and intersection operations.

2、Extrusion, rotation, and other operations based on 2D profiles.

Currently, SharpCAD can export modeling results as OFF and OBJ files. Additionally, you can save your creations as scripts for other scripts to use.

# Coding a simple script?
You can write CSCAD scripts just like writing C# code in Visual Studio. For example, you can create a Box and add it to the scene, and you will see a cube in the scene：

    Box box = new Box(10);
    Scene.Add(box);
![avatar](/images/create_a_box.png)
You can save the script to local path or export it as an OFF/OBJ file.