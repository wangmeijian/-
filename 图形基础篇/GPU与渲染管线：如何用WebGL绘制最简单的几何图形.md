## GPU与渲染管线：如何用WebGL绘制最简单的几何图形？

### 图形系统是如何绘图的？
一个通用计算机图形系统主要包括6个部分，分别是输入设备、中央处理单元、图形处理单元、存储器、帧缓存和输出设备。

* **光栅**（Raster）：几乎所有现代图形系统都是基于光栅来绘制图形的，光栅就是指构成图像的像素阵列
* **像素**（Pixel）：一个像素对应图像上一个点，它通常保存图像上的某个具体位置的颜色等信息
* **帧缓存**（Frame Buffer）：在绘图过程中，像素信息被存放在帧缓存中，帧缓存是一块内存地址
* **CPU**：中央处理单元，负责逻辑运算
* **GPU**：图形处理单元，负责图形计算

数据经过CPU处理，成为具有特定结构的几何信息，然后被送到GPU中进行处理，在GPU中要经过两个步骤生成光栅信息。这些光栅信息会输出到帧缓存中，最后渲染到屏幕上。

GPU 是由大量的小型处理单元构成的，它可能远远没有 CPU 那么强大，但胜在数量众多，可以保证每个单元处理一个简单的任务。即使我们要处理一张 800 * 600 大小的图片，GPU 也可以保证这 48 万个像素点分别对应一个小单元，这样我们就可以同时对每个像素点进行计算了

### 如何用 WebGL 绘制三角形？

#### 步骤一：创建 WebGL 上下文

创建 WebGL 上下文这一步和 Canvas2D 的使用几乎一样，我们只要调用 canvas 元素的 getContext 即可，区别是将参数从’2d’换成’webgl’

```js
const canvas = document.querySelector('canvas');
const gl = canvas.getContext('webgl');
```
#### 步骤二：创建 WebGL 程序

首先，我们要编写两个着色器（Shader）

```js

const vertex = `
  attribute vec2 position;

  void main() {
    gl_PointSize = 1.0;
    gl_Position = vec4(position, 1.0, 1.0);
  }
`;


const fragment = `
  precision mediump float;

  void main()
  {
    gl_FragColor = vec4(1.0, 0.0, 0.0, 1.0);
  }    
`;
```

为什么要创建两个着色器呢？WebGL是以顶点和图元来描述几何信息的，因此，WebGL绘制一个图形的过程，一般要用到两段着色器，一段叫**顶点着色器**（Vertex Shader）负责处理图形的顶点信息，另一段叫**片元着色器**（Fragment Shader）负责处理图形的像素信息

什么是**顶点和图元**？顶点就是几何图形的顶点，比如，三角形有三个顶点。图元是WebGL可直接处理的图形单元。

因为图元是WebGL可以直接处理的图形单元，所以其他非图元的图形最终要转换为图元菜可以被WebGL处理。比如，要绘制实心的四边形，就需要将四边形拆分为两个三角形，再交给WebGL分别绘制出来

以上的顶点着色器和片元着色器只是一段代码，需要将它们分别创建成shader对象

```js

const vertexShader = gl.createShader(gl.VERTEX_SHADER);
gl.shaderSource(vertexShader, vertex);
gl.compileShader(vertexShader);


const fragmentShader = gl.createShader(gl.FRAGMENT_SHADER);
gl.shaderSource(fragmentShader, fragment);
gl.compileShader(fragmentShader);
```

接着，创建 WebGLProgram 对象，并将这两个 shader 关联到这个 WebGL 程序上。WebGLProgram 对象的创建过程主要是添加 vertexShader 和 fragmentShader，然后将这个 WebGLProgram 对象链接到 WebGL 上下文对象上

```js

const program = gl.createProgram();
gl.attachShader(program, vertexShader);
gl.attachShader(program, fragmentShader);
gl.linkProgram(program);
```
最后，通过useProgram启用这个WebGLProgram对象

```js
gl.useProgram(program);
```
