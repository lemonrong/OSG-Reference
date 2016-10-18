osg::Matrix 类保存了一个 4×4 的矩阵，共包含了 16 个浮点数，并提供了 相应的运算操作。Matrix 类不是由 Referenced 派生的，因此不能实现引用计数。

Matrix 类提供的接口与 OpenGL 标准以及大多数 OpenGL 书籍所述有些不同。Matrix提供了与C/C++二维列数组相一致的接口：

```
osg::Matrix m;
m( 0, 1 ) = 0.f; // 设置第二个元素（第 0 行第 1 列）
m( 1, 2 ) = 0.f; // 设置第七个元素（第 1 行第 2 列）
```


OpenGL 矩阵可以表示为一维数组，也就是其相关文档中所述的行数组：

```
⎡ m0 m4 m8  m12 ⎤
⎢ m1 m5 m9  m13 ⎥
⎢ m2 m6 m10 m14 ⎥
⎣ m3 m7 m11 m15 ⎦

GLfloat m;
m[1] = 0.f; // 设置第二个元素
m[6] = 0.f; // 设置第七个元素
```


虽然有这样的区别，OSG和OpenGL矩阵在内存中的存放并不相同——OSG 在向OpenGL送入矩阵参数时不用进行额外的转换。但是对开发者来说，还是有必要在使用各种数据之前记得转换 OSG 的矩阵。

Matrix 提供了完整的向量-矩阵乘法和矩阵级联的功能。要使 Vec3 变量 v沿着新的原点矩阵T，按照矩阵 R执行旋转变换，只需要以下的代码：
```
osg::Matrix T; 
T.makeTranslate( x, y, z );
osg::Matrix R;
R.makeRotate( angle, axis );
Vec3 vPrime = v * R * T;
```
Matrix 采用了左乘（premultiplication）操作，这一点与 OpenGL 文档中所述的 v' = TRv 正好相反。 

上述  OpenGL  运算得到的结果是相同的，因为OpenGL 的行数组矩阵采用右乘的方式，而OSG的列数组矩阵采用左乘的方式，这两者是等价的。
如果要对一个 Geode使用上述变换，首先应该创建一个包含矩阵 T 的 MatrixTransform 节点，向其添加一个包含矩阵 R 的 MatrixTransform子节点，然后再向这个旋转变换节点添加子节点 Geode，如图所示。这与下面的OpenGL语句是等价的：
```
glMatrixMode( GL_MODELVIEW ); 
glTranslatef( ... ); // 变换矩阵 T 
glRotatef( ... ); // 旋转矩阵 R
...
glVertex3f( ... );
glVertex3f( ... );
...
```
总的说来，OSG 的列数组接口与OpenGL的行数组接口有所不同，但是它们在内部采用了等价的运算操作，因此它们的矩阵是100%兼容的。
