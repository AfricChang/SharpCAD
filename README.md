# SharpCAD
SharpCAD is a free software that creates 3D objects using scripts, similar to OpenSCAD. However, it uses C# scripts and currently only supports Windows. Although it is currently only a free software and not open source, it will be open sourced in the future.
Like OpenSCAD, SharpCAD provides two main modeling techniques:

1、Constructive Solid Geometry (CSG): SharpCAD defines 2D objects such as Circle and Square, as well as 3D basic objects like Box and Sphere. However, unlike OpenSCAD, you need to use +, -, and * operators for Boolean union, difference, and intersection operations.

2、Extrusion, rotation, and other operations based on 2D profiles.

Currently, SharpCAD can export modeling results as OFF and OBJ files. Additionally, you can save your creations as scripts for other scripts to use.

# Coding a simple script?
You can write CSCAD scripts just like writing C# code in Visual Studio. For example, you can create a Box and add it to the scene, and you will see a cube in the scene：

    Box box = new Box(10);
    Scene.Add(box);
![avatar](images/create_a_box.png)
You can save the script to local path or export it as an OFF/OBJ file.


# Contents
- [3rd-party libraries](#3rd-party-libraries)
- [Examples](#Examples)
- [Done and TODO](#Done-and-TODO)
- [Reference Table](#Reference-Table)

# 3rd-party libraries
* [FastColoredTextBox](https://github.com/PavelTorgashov/FastColoredTextBox) 
* [Newtonsoft.Json](https://github.com/JamesNK/Newtonsoft.Json/)
* [scriptcs-engines](https://github.com/scriptcs/scriptcs-engines)
* [Open Cascade](https://www.Opencascade.com/)
* [AvalonEdit](https://github.com/icsharpcode/AvalonEdit)
* [Autofac](https://github.com/autofac/Autofac)
* [CShell](https://github.com/lukebuehler/CShell)
* [MPFR](https://www.mpfr.org/)
* [CGAL](https://www.cgal.org/)
* [GMP](https://gmplib.org/)  
* [TBB](https://github.com/oneapi-src/oneTBBhttps://gmplib.org/)  

# Examples
目前只提供了9个示例，所有示例都在examples目录下

# Done and TODO
## 相对OpenSCAD已支持
* 二维图元已支持，Text未支持，后续考虑支持
* projection cut参数未支持，后续考虑支持
* linear_extrude部分支持，twist , slices , scale 参数未支持，后续可能会支持，其余参数不考虑支持
* rotate_extrude,$fn没支持，但在三角化时可知设置dAngleDelta，后续考虑在导出时或渲染时支持该参数
* import，目前仅支持OFF，后续会考虑支持OBJ、STL、DXF等其他格式
* 布尔运算已支持
## 后续计划
* 上述OpenSCAD参数完善
* Text支持
* 镜像
* 扫掠
* 放样
* 平面分割对象
* 平面与对象相交
* 更多的示例
* 自定义插件
* ......

# Reference Table
目前所有建模操作都在ACC命名空间都OCCTModeling.dll).
ACC命名空间提供了基础的Point2D、Point3D、几何图元及相关运算。
## 几何图元
包括二维及三维几何图元，所有的二三维图元(除了TriangleMesh)都继承自ShapeBase，都可以进行旋转(Rotate方法)、平移(Translate方法)、缩放(Scale方法)、投影以及布尔运算(通过符号+、-、*实现)，其中二维图元还可以进行拉伸、旋转拉伸。非三角网的图元可以三角化后导出。

### Circle
    二维圆
    Circle(double radius)//创建指定半径的圆

### Square
    方形
    Square(double height, double width, bool bCenter)//指定长宽及中心点是否在原点
    Square(double height, double width)//指定长宽及中心点在原点
    Square(double size, bool bCenter))//长宽均等于size中心点是否在原点
    Square(double size)//长宽均等于size中心点在原点

### PolyFace
    多边形
    PolyFace(List<Point3D> verteces, List<List<int>> indeces)//指定顶点及索引创建带洞多边形，注意，支持带洞多边形，第一个面需要逆时针，后续面作为洞应顺时针。
    PolyFace(List<Point3D> verteces)//简单多边形

### Box
    立方体
    Box(double dLength, double dWidth, double dHeight, bool bCenter)//指定长宽高以及中心点是否在原点
    Box(double dLength, double dWidth, double dHeight)//指定长宽高,中心点在原点
    Box(double size, bool bCenter)//长宽高均为size，中心点是否在原点
    Box(double size)//长宽高均为size，中心点在原点

### Sphere
    球体
    Sphere(double dRadius)//指定半径创建球体

### Cylinder
    圆柱体
    Cylinder(double dTopRadius, double dBottomRadius, double dHeight)//指定顶面半径、底面半径以及高创建圆柱体
    Cylinder(double dRadius, double dHeight)//创建顶底半径相等高为dHeight的圆柱体

### Torus
    圆环
    Torus(double dRadius, double dRingRadius, double dSweepAngle)//指定圆半径、管半径以及扫掠角度，注意，扫掠角度是以X轴为起点，逆时针扫过的角度，以度为单位

### TriangleMesh
    三角网
    导入的OFF(目前仅支持OFF,后续会支持OBJ、STL等其他格式)以及几何图元三角化之后的结果。

## 运算
所有的运算都在ACC.ACOCCTTools内中,主要包含旋转、平移、缩放、投影、布尔运算等操作

### 变换操作
* public static void Translate(ShapeBase shape, double x, double y, 
    double z)//平移图元
* public static void Translate(ShapeBase shape, Point3D translate)//平移图元
* public static void Rotate(ShapeBase shape, double x, double y, 
    double z)//旋转图元，以度为单位
* public static void Rotate(ShapeBase shape, Point3D rotate)//旋转图元，以度为单位
* public static void Scale(ShapeBase shape, double x, double y, 
    double z)//缩放图元
* public static void Scale(ShapeBase shape, Point3D scale)//缩放图元

### 运算操作
* public static TriangleMesh BooleanOperation(TriangleMesh triMeshA, TriangleMesh triMeshB, BooleanType eBoType)//三角网布尔运算，BooleanType包含交(Intesection)、并(Union)、差(Diffrence)三种。注意，如果ShapeBase图元和三角网图元布尔运算需要先将ShapeBase三角化为三角网图元才可运算
    
* public static TriangleMesh Trianguation(ShapeBase shape, double dAngleDelta, double dLengthDelta)//三角化ShapeBase图元,dAngleDelta为角度阈值，dLengthDelta为长度阈值。

* public static ShapeBase Linear_Extrude(ShapeBase shape, double height)//拉伸图元

* public static ShapeBase Rotate_Extrude(ShapeBase shape, double dAngle)//旋转拉伸图元

* public static TriangleMesh Import(string strFileName)//导入文件，目前仅支持OFF

* public static List<PolyFace> Projection(ShapeBase shape)//图元投影
* public static List<PolyFace> Projection(TriangleMesh triMesh)//三角网投影
* public static Combine(System.Collections.Generic.List<ACC.TriangleMesh>)//合并三角网
