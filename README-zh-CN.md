# SharpCAD
SharpCAD是一个用脚本创建三维对象的免费软件，像OpenSCAD一样，不过它使用的脚本是C#，且目前仅支持windows。现在还仅仅是免费软件，还未开源，不过以后我会将其开源。

和OpenSCAD一样，SharpCAD提供了两种主要的建模技术:

1、构造实体几何=>CSG。SharpCAD定义了Circle、Square二维对象以及Box、Sphere等三维基础对象。不过和OpenSCAD不一样的是，你需要用+、-、*来进行布尔并、差、交运算。

2、基于二维轮廓的拉伸旋转等操作。

目前SharpCAD可将建模成果导出为OFF以及OBJ文件，当然你也可以把你的成果保存为脚本供其他脚本调用。

# 编写一个简单的脚本吧
你可像在Visual Studio里编写C#代码一样编写CSCAD脚本。
比如创建一个Box然后添加到场景中你就会在场景看到一个立方体:

    Box box = new Box(10);
    Scene.Add(box);

![avatar](/images/create_a_box.png)
然后保存脚本到本地或导出为OFF/OBJ文件。

# 目录
- [用到的三方库](#用到的三方库)
- [现有示例脚本](#现有示例脚本)
- [已支持及后续计划](#已支持及后续计划)
- [函数速查表](#函数速查表)

# 用到的三方库
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

# 现有示例脚本
目前只提供了9个示例，所有示例都在examples目录下

# 已支持及后续计划
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

# 函数速查表
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
