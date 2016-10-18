虽然 PrimitiveSet 子类提供了与OpenGL顶点数组特性几乎等价的功能，但是不要认为 PrimitiveSet在内部机理上也是如此使用顶点数组的。根据渲染环境的不同，OSG可能会使用顶点数组（附加或不附加缓冲对象），显示列表，甚至glBegin()/glEnd()来渲染几何体。 继承自 Drawable类的对象（例如Geometry）在缺省条件下将使用显示列表。你也可以调用 osg:: Drawable:: setUseDisplayList( false )来 改变这一特性。

如果用户设置了 BIND_PER_PRIMITIVE 这种绑定方式，那么 OSG   将 依 赖   glBegin()/glEnd() 函 数 进 行 渲 染 。 BIND_PER_PRIMITIVE方式为每个独立的几何图元设置一种绑定属性（例如，为 GL_TRIANGLES 中的每个三角形）。
