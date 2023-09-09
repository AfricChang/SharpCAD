# SharpCAD
SharpCAD是一个用脚本创建三维对象的免费软件，像OpenSCAD一样，不过它使用的脚本是C#，且目前仅支持windows。现在还仅仅是免费软件，还未开源，不过说不定以后我会将其开源。

和OpenSCAD一样，SharpCAD提供了两种主要的建模技术:

1、构造实体几何=>CSG。SharpCAD定义了Circle、Square二维对象以及Box、Sphere等三维基础对象。不过和OpenSCAD不一样的是，你需要用+、-、*来进行布尔并、差、交运算。

2、基于二维轮廓的拉伸旋转等操作。

目前SharpCAD可将建模成果导出为OFF以及OBJ文件，当然你也可以把你的成果保存为脚本供其他脚本调用。

# 编写一个简单的脚本吧
你可像在Visual Studio里编写C#代码一样编写CSCAD脚本。
比如创建一个Box然后添加到场景中你就会在场景看到一个立方体:

    Box box = new Box(10);
    Scene.Add(box);

![avatar](github.com/AfricChang/SharpCAD/tree/main/images/create_a_box.png)
然后保存脚本到本地或导出为OFF/OBJ文件。

