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

