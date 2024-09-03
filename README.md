1、环境配置(Environment Setup)
Hazel 使用 C++ 编写，并且依赖于一些外部库，如 GLFW（用于窗口管理和输入处理）、GLAD（用于 OpenGL 函数加载）、ImGui（用于用户界面）、glm（用于数学计算）以及 spdlog（用于日志记录）

2、引擎架构设计 (Engine Architecture)
核心（Core）：提供基础的引擎功能，如日志记录、输入系统，基础数据结构，时间管理、层，窗口定义后通过glfw，应用整合所有功能。
事件系统 (Event System)：用于处理窗口输入事件和引擎内部事件。
图形 (Renderer)：负责所有的图形渲染功能：正交相机，着色器，纹理，后面主要通过OpenGL 实现。
小窗ImGui：实现窗口的交互。控制窗口
沙盒应用（Sandbox）：用户创建应用，基于layer层的概念来管理引擎和用户应用代码的分离。

3、 窗口和事件处理 (Window and Event Handling)
Hazel 使用 GLFW 来创建和管理窗口，并处理用户输入事件（如键盘和鼠标，设置新的尺寸）。GLFW 是一个轻量级的库，支持 OpenGL 和 Vulkan 渲染 API。
Window 类：封装了 GLFW 窗口的创建和管理。它负责初始化 OpenGL 上下文、设置窗口属性、以及窗口的销毁。
Event 系统：采用观察者模式（Observer Pattern），事件（如鼠标点击、窗口关闭）会分发到订阅这些事件的层。

4、   事件处理 (Event Handling))
Hazel 的事件系统采用了一个基于观察者模式的设计。所有的事件（如窗口关闭、鼠标移动）都会被分发到订阅这些事件的层。
事件类 (Event)：
提供了一个基类，所有特定的事件（如 KeyEvent、MouseEvent）都从这个基类继承。

5、 OpenGL 渲染 (OpenGL Rendering)
Hazel 当前使用 OpenGL 作为主要的图形 API。Hazel 的渲染器设计分为几个关键部分：
Renderer 类：负责初始化渲染管道，设置渲染状态，并控制渲染流程，开启混合纹理混合，设置相机视角。
VertexArray、VertexBuffer、IndexBuffer 类：封装 OpenGL 的缓冲区对象，便于数据的管理和传输。
Shader 类：封装 OpenGL 着色器的创建、编译、链接和使用。
Context类：将 OpenGL 上下文绑定到当前线程并且获取硬件信息。
Texture 类：读取纹理图片。

6、  输入系统 (Input System)
Hazel 的输入系统基于 GLFW，并提供了一套简单的 API 来处理键盘和鼠标输入。
功能：
键盘输入：检测特定键是否被按下。
鼠标输入：处理鼠标按钮、滚轮和位置。
事件处理：将输入事件分发到对应的层或回调函数。


7、 相机控制 (Camera Control)
Hazel 使用一个简单的相机类来控制视角。相机通常用于窗口的视图设置，它包含了投影矩阵和视图矩阵的管理。
相机类型：
	• 正交相机（Orthographic Camera）：用于2D渲染，提供平行投影。
相机控制：通过捕获用户输入（如鼠标移动和键盘按键），调整相机的位置和方向。

8、  用户界面 (User Interface)
Hazel 使用 ImGui 来实现用户界面。ImGui 是一个非常流行的即时模式 GUI 库，适合用于开发工具和调试界面。
功能：
UI 元素：如按钮、滑块、文本框等。
调试工具：如帧率显示、调试控制台等。


9、  图层管理 (Layer Management)
Hazel 使用图层（Layer）来管理引擎的各个部分和用户代码。图层堆栈可以动态调整，以便于管理不同的渲染和事件处理逻辑。
图层堆栈 (LayerStack)：
提供了一个栈结构来管理图层的添加、删除和排序。支持首部和尾部插入Layer。


10、  纹理 (Textures)
Hazel 使用 OpenGL 来加载和管理纹理。纹理是图形渲染中非常重要的一部分，用于在几何体上绘制图像。
Texture 类：
封装纹理的加载、生成和绑定过程。支持从文件中加载纹理，或动态生成纹理。


11、 着色器编程 (Shader Programming)
Hazel 使用 GLSL 进行着色器编程。着色器在 Hazel 中被封装在 Shader 类中，这个类提供了加载、编译、和管理着色器程序的方法。
Shader 类的功能：负责初始化渲染管道，设置渲染状态，并控制渲染流程，开启混合纹理混合，设置相机视角。
VertexArray、VertexBuffer、IndexBuffer 类：封装 OpenGL 的缓冲区对象，便于数据的管理和传输。
Shader 类的功能：
加载 GLSL 源码：从文件中读取顶点和片段着色器的 GLSL 代码。
编译和链接：编译 GLSL 代码并将其链接成一个完整的着色器程序。
Uniform 管理：提供方法将数据传递给着色器，如矩阵、纹理和颜色。


12、  资源管理 (Resource Management)
Hazel 采用了一种简单的资源管理策略，主要是通过智能指针（如 std::shared_ptr）和资源缓存（如着色器缓存、纹理缓存）来管理资源的加载和使用。
资源缓存：
确保每个资源只加载一次，并在需要时重复使用，从而减少内存使用和加载时间。

13、 2D 渲染 (2D Rendering)
Hazel 支持基本的2D渲染功能，使用正交相机和简单的几何体（如四边形）来渲染2D场景。
关键类：
Renderer2D：提供2D渲染的功能，如绘制纹理化四边形和线条。
![Uploading image.png…]()
