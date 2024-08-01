# SIBR Project

## Introduction

### 导言

The SIBR project consists of more than 246 files and is structured into several main modules, as illustrated in the architecture diagram below.

SIBR 项目包含超过 246 个文件，结构划分为多个主要模块，如下方的架构图所示。

![Introduction](presentation/2.jpg)

---

## Project Structure

### system

The `core/system` module contains tools related to the operating system, such as system files and file manipulation tools. It also includes a configuration file (`Config.hpp`) that defines several useful macros and settings.

`core/system` 模块包含与操作系统相关的工具，如系统文件和文件操作工具。它还包括一个配置文件（`Config.hpp`），该文件定义了许多有用的宏和设置。

### graphics

The `core/graphics` module encompasses elements related to graphics, including images, textures, and render targets. It uses OpenCV for image management and operations. Note that `sibr::Image` encapsulates `cv::Mat` to handle images.

`core/graphics` 模块涵盖与图形相关的元素，包括图像、纹理和渲染目标。它使用 OpenCV 进行图像管理和操作。请注意，`sibr::Image` 封装了 `cv::Mat` 用于处理图像。

### assets

The `core/assets` module manages basic resources for IBR rendering. These resources are essential for loading and reading various types of data files.

`core/assets` 模块管理 IBR 渲染的基本资源。这些资源对于加载和读取各种类型的数据文件至关重要。

### scene

The `core/scene` module represents IBR scenes, allowing for the management and storage of complex data. A notable example is `sibr::BasicIBRScene`, which initializes the necessary components for scenes.

`core/scene` 模块表示 IBR 场景，允许管理和存储复杂的数据。一个显著的例子是 `sibr::BasicIBRScene`，它初始化了场景所需的组件。

### raycaster

The `core/raycaster` module provides tools for managing rays and performing intersection tests, leveraging the Embree library for fast execution.

`core/raycaster` 模块提供了用于管理射线和执行相交测试的工具，利用 Embree 库实现快速执行。

### imgproc

The `core/imgproc` module is dedicated to basic image processing, with OpenCV support for more complex tasks.

`core/imgproc` 模块专注于基本的图像处理，支持使用 OpenCV 进行更复杂的任务。

### video

The `core/video` module handles video loading and processing, relying on ffmpeg internally.

`core/video` 模块处理视频的加载和处理，内部依赖于 ffmpeg。

### view

The `core/view` module provides tools for visual scene rendering, with a basic user interface and interactive camera modes.

`core/view` 模块提供了用于视觉场景渲染的工具，具有基本的用户界面和交互式摄像机模式。

### renderer

The `core/renderer` module encapsulates rendering functionalities for IBR applications, offering a simple interface for graphic display.

`core/renderer` 模块封装了 IBR 应用程序的渲染功能，提供了一个简单的图形显示接口。

![Structure](presentation/3.jpg)

---

## Rendering Pipeline

The rendering pipeline in SIBR is broken down into several stages:

SIBR 中的渲染管道分为几个阶段：

1. **Command line (命令行)**: Launching the player from the command line.
2. **Inputs (输入)**: Storing the inputs into variables for later use.
3. **Rendering (渲染过程)**: The rendering process based on the provided inputs.
4. **2D image (二维图像)**: Producing the final 2D image as the result of the rendering.
   

1. **Command line (命令行)**：从命令行启动播放器。
2. **Inputs (输入)**：将输入存储到变量中以供以后使用。
3. **Rendering (渲染过程)**：基于提供的输入进行渲染过程。
4. **2D image (二维图像)**：生成最终的二维图像作为渲染的结果。

![Pipeline](presentation/4.jpg)

---
## Project Structure (continued)

### Files

The SIBR project involves several key files and directories that structure the data and configuration needed for the rendering process.

SIBR 项目包含几个关键文件和目录，这些文件和目录构成了渲染过程所需的数据和配置。

![Structure](presentation/5.jpg)

- **point_cloud**: Contains the point cloud data used for rendering.
  - **cameras**: Directory storing camera configuration files.
  - **cfg_args**: Configuration arguments for the rendering process.
  - **input**: Directory holding input files for the rendering pipeline.

  **point_cloud**: 包含用于渲染的点云数据。
  - **cameras**: 存储相机配置文件的目录。
  - **cfg_args**: 渲染过程的配置参数。
  - **input**: 保存渲染管道输入文件的目录。


---

## Rendering Pipeline (continued)

The rendering pipeline further involves the following stages:

渲染管道还包括以下阶段：

1. **Command line (命令行)**:
   - Path of our folder: 文件夹的路径
   - Store command line in a variable: 将命令行存储在变量中
   - Set different options: 设置不同的选项
   - Store point cloud path in variable: 在变量中存储点云的路径

![Pipeline](presentation/6.jpg)

---

## Command Line (continued)

Further details on using the command line in SIBR:

有关在 SIBR 中使用命令行的更多详细信息：

1. **Command line (命令行)**:
   - Path of our folder: 文件夹的路径
   - Store command line in a variable: 将命令行存储在变量中
   - Set different options: 设置不同的选项
   - Store point cloud path in variable: 在变量中存储点云的路径

![Command Line](presentation/7.jpg)


---

## Rendering Pipeline (continued)

The rendering pipeline further involves the following stages:

渲染管道还包括以下阶段：

1. **Inputs (输入)**：将输入存储到变量中以供以后使用。

![Pipeline](presentation/8.jpg)

---
## Load the Files (continued)

This stage of the pipeline involves creating the scene and loading the necessary files.

这一阶段涉及创建场景和加载必要的文件。

1. **Create the scene (创建场景)**:
   - Create the mesh (in proxy) with `input.ply`: 使用 `input.ply` 创建网格（代理中）

2. **Gaussian View (高斯视图)**:
   - Create the ULR view and store the `point_cloud.ply` in variables: 创建 ULR 视图并在变量中存储 `point_cloud.ply`

![Load the Files](presentation/9.jpg)
---

## Rendering Pipeline (continued)

The rendering pipeline further involves the following stages:

渲染管道还包括以下阶段：

1. **Rendering (渲染过程)**：基于提供的输入进行渲染过程。

![Pipeline](presentation/10.jpg)
---

## Rendering Process (continued)


![Rendering Process](presentation/12.jpg)

This section outlines the detailed steps involved in the rendering process, including various operations such as checking the render mode, converting view and projection matrices, and handling CUDA operations.

此部分概述了渲染过程中涉及的详细步骤，包括检查渲染模式、转换视图和投影矩阵以及处理 CUDA 操作。

1. **Render Mode Check (渲染模式检查)**:
   - If the current mode is "Ellipsoids," the `gaussianRenderer->process` method is called to process the ellipsoids.
   - If the current mode is "Initial Points," the `pointbasedrenderer->process` method is called to process the initial points.

2. **Conversion of View and Projection Matrices (视图和投影矩阵的转换)**:
   - Convert the view and projection to the target coordinate system.

3. **Calculation of Additional View Parameters (附加视图参数的计算)**:
   - Compute additional view parameters like `tan_fovy` and `tan_fovx`.

4. **Copy Frame-Dependent Data to GPU (将帧相关数据复制到 GPU)**:
   - Copy necessary data like view and projection matrices to GPU memory.

5. **Mapping of the OpenGL Resource for use with CUDA (映射 OpenGL 资源，以便与 CUDA 配合使用)**:
   - Map the OpenGL resource to make it accessible for CUDA operations.

6. **Rasterization (光栅化)**:
   - Rasterize Gaussian splats into `image_cuda`.

7. **Unmapping the OpenGL Resource for use with OpenGL (取消映射 OpenGL 资源，以便与 OpenGL 配合使用)**:
   - Unmap the OpenGL resource, making it available for use with OpenGL.

8. **Copy Image Content to Framebuffer (将图像内容复制到帧缓冲区)**:
   - Copy the final image data to the framebuffer for display.

9. **CUDA Error Handling (CUDA 错误处理)**:
   - Manage any potential errors that occur during CUDA operations.

![Rendering Process](presentation/13.jpg)
-----

## Scaffold Differences (差异)

This section introduces the differences related to the scaffold in the SIBR project. The term "scaffold" here refers to the structure or framework used to organize and process the rendering data.

本节介绍了 SIBR 项目中与脚手架相关的差异。这里的“脚手架”一词是指用于组织和处理渲染数据的结构或框架。

![Scaffold Differences](presentation/15.jpg)
---
