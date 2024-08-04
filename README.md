# SIBR Project

## Introduction

### 导言

The SIBR project consists of more than 246 files and is structured into several main modules, as illustrated in the architecture diagram below.

SIBR 项目包含超过 246 个文件，结构划分为多个主要模块，如下方的架构图所示。

![Introduction](presentation/2.jpg)

The project is quite complex, but not all files need to be modified. Only the most important ones require changes. Specifically, some functions that store our `point_cloud.ply` file need to be adapted. Additionally, the functions responsible for rendering the Gaussian cloud based on the characteristics we have chosen to retain or discard must be modified. Lastly, other files related to efficient computation on the GPU (coded in CUDA) also require adjustments.

这个项目相当复杂，但并不是所有文件都需要修改。只有最重要的文件需要更改。具体来说，存储我们 `point_cloud.ply` 文件的某些函数需要进行调整。此外，负责根据我们选择保留或丢弃的特性渲染高斯点云的函数也必须修改。最后，与 GPU 上高效计算相关的其他文件（用 CUDA 编写）也需要进行调整。


---

## Table of Contents

- [Introduction](#introduction)
  - [导言](#导言)
- [Project Structure](#project-structure)
  - [项目结构](#project-structure)
- [Rendering Pipeline](#rendering-pipeline)
  - [渲染过程](#rendering-process)
- [Scaffold Differences](#scaffold-differences)
  - [脚手架差异](#scaffold-differences)
- [Differences in Spacetime Method](#differences-in-spacetime-method)
  - [时空方法的差异](#differences-in-spacetime-method)
- [Conclusion](#Conclusion)
  - [结论](#Conclusion)

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

1. **Command line (命令行)**: Launching the player from the command line. 从命令行启动播放器。
2. **Inputs (输入)**: Storing the inputs into variables for later use. 将输入存储到变量中以供以后使用。
3. **Rendering (渲染过程)**: The rendering process based on the provided inputs. 基于提供的输入进行渲染过程。
4. **2D image (二维图像)**: Producing the final 2D image as the result of the rendering. 生成最终的二维图像作为渲染的结果。


![Pipeline](presentation/4.jpg)

Now, let's delve into the pipeline of this project. We will start by looking at the overall concept and situation, and then gradually move into more specific details. 

接下来，我们将深入分析该项目的管道。我们将首先从整体概念和情况入手，然后逐渐深入到更具体的细节中。

First, we will analyze the basic structure of the viewer for the Gaussian splatting method. Then, we will examine the differences between the Scaffold and Spacetime methods. It's important to note that both of these methods build upon the base player (Gaussian-splatting) and introduce certain modifications to it.

首先，我们将分析用于高斯点云方法的查看器的基本结构。然后，我们将研究 Scaffold 和 Spacetime 方法之间的差异。需要注意的是，这两种方法都是在基本播放器（高斯点云）基础上进行修改的。

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


Recommended checkpoint  structure in the model path location:

```
<location>
|---point_cloud
|   |---point_cloud.ply
|---cfg_args
|---cameras.json
(|---input.ply)
```


- **Configuration Arguments (cfg_args)**: While not essential, this file allows customization options for the player, such as background color adjustments. It's a way to enhance the user experience based on specific needs or preferences, such as enabling a white background.
  - **配置参数 (cfg_args)**：尽管不是必需的，这个文件允许为播放器添加自定义选项，例如背景颜色调整。它是一种根据特定需求或偏好增强用户体验的方法，例如启用白色背景。

- **Point Cloud Data (point_cloud.ply)**: This file is the cornerstone of the rendering process, holding detailed information about each point in the Gaussian point cloud, such as position, color, and opacity. These attributes are crucial for accurate rendering and will be utilized extensively in subsequent processes.
  - **点云数据 (point_cloud.ply)**：这个文件是渲染过程的基石，包含了关于高斯点云中每个点的详细信息，例如位置、颜色和不透明度。这些属性对于准确渲染至关重要，并将在后续过程中广泛使用。


---

## Rendering Pipeline (continued)

The rendering pipeline further involves the following stages:

渲染管道还包括以下阶段：

1. **Command line (命令行)**:

![Pipeline](presentation/6.jpg)



## Command Line (continued)

Further details on using the command line in SIBR:

有关在 SIBR 中使用命令行的更多详细信息：

1. **Command line (命令行)**:
   - Path of our folder: 文件夹的路径
   - Store command line in a variable: 将命令行存储在变量中
   - Set different options: 设置不同的选项
   - Store point cloud path in variable: 在变量中存储点云的路径

![Command Line](presentation/7.jpg)

After building the project from the source code, not everything works as expected. Some small adjustments are necessary. I used MobaXterm to get a graphical interface that connects through SSH, but the simplest and fastest solution is to use a Windows machine with a good graphics card. The problem with doing it remotely is that the FPS (frames per second) are very low.

从源代码构建项目后，并非所有内容都按预期工作。需要进行一些小的调整。我使用了 MobaXterm 获取通过 SSH 连接的图形界面，但最简单、最快速的解决方案是使用具有良好显卡的 Windows 机器。远程操作的问题在于 FPS（每秒帧数）非常低。

It's crucial to ensure that all the versions required are exactly the same as those specified on the Gaussian-splatting GitHub repository (e.g., Visual Studio 2019, CUDA 11.8, etc.). Otherwise, you may encounter numerous issues when attempting to build the project.

确保所有所需版本与 Gaussian-splatting GitHub 仓库中指定的版本完全一致（例如，Visual Studio 2019、CUDA 11.8 等）至关重要。否则，在构建项目时可能会遇到许多问题。

Here is the command that successfully launched the viewer:

以下是成功启动查看器的命令：

```bash
cd path-to-directory/gaussian-splatting/SIBR_viewers/install/shaders/core
```
```bash
MESA_GL_VERSION_OVERRIDE=4.5 ../../bin/SIBR_gaussianViewer_app -m "path-to-your-model"
```

---

## Rendering Pipeline (continued)

The rendering pipeline further involves the following stages:

渲染管道还包括以下阶段：

1. **Inputs (输入)**：将输入存储到变量中以供以后使用。

![Pipeline](presentation/8.jpg)

---
## Load the Files (continued)

### This stage of the pipeline involves creating the scene and loading the necessary files.

这一阶段涉及创建场景和加载必要的文件。

1. **Create the scene (创建场景)**:
   - Create the mesh (in proxy) with `input.ply`: 使用 `input.ply` 创建网格（代理中）

2. **Gaussian View (高斯视图)**:
   - Create the ULR view and store the `point_cloud.ply` in variables: 创建 ULR 视图并在变量中存储 `point_cloud.ply`

![Load the Files](presentation/9.jpg)

`input.ply` is not very important and can be removed without significantly affecting the final rendering. What is crucial is `point_cloud.ply`, and the `loadPly` function in `gaussianView.cpp` is responsible for storing the information in appropriate variables for each Gaussian.

`input.ply` 不是很重要，可以删除，对最终渲染不会有太大影响。关键是 `point_cloud.ply`，而 `gaussianView.cpp` 中的 `loadPly` 函数负责将每个高斯的相关信息存储在适当的变量中。

`loadPly` is therefore an important function that will need to be modified in the new method and adapted according to the information we want to store.

因此，`loadPly` 是一个相当重要的函数，在新方法中需要对其进行修改，并根据我们想要存储的信息进行适配。

---

## Rendering Pipeline (continued)

The rendering pipeline further involves the following stages:

渲染管道还包括以下阶段：

1. **Rendering (渲染过程)**：基于提供的输入进行渲染过程。

![Pipeline](presentation/10.jpg)
---

## Rendering Process (continued)


在 `main.cpp` 的主循环中：`onRender` 函数调用其他渲染函数，而这些渲染函数又调用其他渲染函数，最终调用 `onRenderIBR`。

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

`onRenderIBR` is therefore an important function that will also need to be modified in `GaussianView.cpp`. We will examine in more detail how it could be modified by drawing inspiration from the viewers of Spacetime and Scaffold.

因此，`onRenderIBR` 是一个重要的函数，也需要在 `GaussianView.cpp` 中进行修改。我们将更详细地研究如何借鉴 Spacetime 和 Scaffold 的查看器对其进行修改。


----

## Scaffold Differences

![Scaffold Differences](presentation/15.jpg)

This section introduces the differences related to the scaffold in the SIBR project. The term "scaffold" here refers to the structure or framework used to organize and process the rendering data.

本节介绍了 SIBR 项目中与脚手架相关的差异。这里的“脚手架”一词是指用于组织和处理渲染数据的结构或框架。

---

## Differences in Scaffold (差异)

This section highlights the differences between the original setup and the scaffold setup in the SIBR project, particularly focusing on the `point_cloud` and `cfg_args` files.

本节重点介绍了 SIBR 项目中原始设置与脚手架设置之间的差异，特别是 `point_cloud` 和 `cfg_args` 文件的差异。

Recommended checkpoint structure in the model path location for the Scaffold method:

建议在模型路径位置使用以下脚手架方法的检查点结构：

```
<location>
|---point_cloud
|   |---point_cloud.ply
|   |---color_mlp.pt
|   |---cov_mlp.pt
|   |---opacity_mlp.pt
(|   |---embedding_appearance.pt)
|---cfg_args
|---cameras.json
(|---input.ply)
```

## Differences Between Original and Scaffold Setup

本节重点介绍了 SIBR 项目中原始设置与脚手架设置之间的差异，特别是 `point_cloud` 和 `cfg_args` 文件的差异。

- **Original (原始)**:
  - The original `point_cloud` file contains various properties like vertex positions, normal vectors, and colors.
  - The `cfg_args` file in the original setup specifies paths, resolution, and other rendering parameters.

- **Scaffold (脚手架)**:
  - The scaffold `point_cloud` file includes additional properties such as offsets and anchor features.
  - The `cfg_args` file in the scaffold setup includes more complex parameters, allowing for additional customization in the rendering process.

In the scaffold method, MLPs (Multi-Layer Perceptrons) are used to retrieve features of the Gaussians (such as color, covariance, opacity, etc.), so these features also need to be stored in appropriate variables.

- **Original (原始)**:
  - 原始的 `point_cloud` 文件包含各种属性，如顶点位置、法向量和颜色。
  - 在原始设置中，`cfg_args` 文件指定了路径、分辨率和其他渲染参数。

- **Scaffold (脚手架)**:
  - 在脚手架设置中，`point_cloud` 文件包含额外的属性，如偏移量和锚点特性。
  - 在脚手架设置中，`cfg_args` 文件包含更复杂的参数，允许在渲染过程中进行更多自定义。

在脚手架方法中，使用 MLP（多层感知器）来提取高斯特性（如颜色、协方差、不透明度等），因此这些特性也需要保存在适当的变量中。


![Differences](presentation/16.jpg)


## Differences in `main.cpp` (差异)

This section highlights the differences in the `main.cpp` file between the original and scaffold setups in the SIBR project.

本节重点介绍了 SIBR 项目中 `main.cpp` 文件在原始设置和脚手架设置之间的差异。

- **Original (原始)**:
  - The original code handles basic configuration arguments like `source_path`, `sh_degree`, and `white_background`.
  - The creation of the ULR view is straightforward, without additional parameters.

- **Scaffold (脚手架)**:
  - The scaffold version includes additional configuration arguments such as `add_opacity_dist`, `add_cov_dist`, and `add_color_dist`, allowing for more detailed customization.
  - The ULR view creation in the scaffold setup also passes additional parameters related to these new configuration options.
 
- **Original (原始)**:
  - 原始代码处理基本的配置参数，如 `source_path`、`sh_degree` 和 `white_background`。
  - ULR 视图的创建很简单，没有额外的参数。

- **Scaffold (脚手架)**:
  - 脚手架版本包含额外的配置参数，如 `add_opacity_dist`、`add_cov_dist` 和 `add_color_dist`，允许进行更详细的自定义。
  - 在脚手架设置中创建 ULR 视图时，也会传递与这些新配置选项相关的额外参数。


![Differences in main.cpp](presentation/17.jpg)

## Differences in `GaussianView.cpp`

### Differences in `RichPoint` and `AnchorPoint` Structs 

This section highlights the differences in the `GaussianView.cpp` file, particularly focusing on the `RichPoint` and `AnchorPoint` structs, as well as the parameter list and constructor in the original versus the scaffold version.

本节重点介绍了 `GaussianView.cpp` 文件中的差异，特别是 `RichPoint` 和 `AnchorPoint` 结构体，以及原始版本和脚手架版本中的参数列表和构造函数。

- **Original (原始)**:
  - The `RichPoint` struct includes basic properties like position, scale, and rotation.
  - The parameters and constructor are simpler, with fewer attributes.

- **Scaffold (脚手架)**:
  - The `AnchorPoint` struct introduces additional properties such as normal, offset, and features, allowing for more complex rendering.
  - The parameters and constructor are extended to include these additional properties, providing more flexibility in the rendering process.
 
- **Original (原始)**:
  - `RichPoint` 结构体包含基本属性，如位置、缩放和旋转。
  - 参数和构造函数更简单，属性较少。

- **Scaffold (脚手架)**:
  - `AnchorPoint` 结构体引入了额外的属性，如法向量、偏移量和特征，使渲染更加复杂。
  - 参数和构造函数进行了扩展，以包含这些额外的属性，从而在渲染过程中提供了更大的灵活性。


![Differences in GaussianView.cpp](presentation/19.jpg)



## Differences in `onRenderIBR` Function 

### Steps Comparison in `onRenderIBR` Function

This section compares the steps involved in the `onRenderIBR` function between the original and scaffold versions in the `GaussianView.cpp` file. The scaffold version includes more detailed operations for handling and processing the Gaussian splats.

本节比较了 `GaussianView.cpp` 文件中 `onRenderIBR` 函数的原始版本和脚手架版本中的步骤。脚手架版本包括更多详细的操作，用于处理和处理高斯斑点。

- **Original (原始)**:
  - The original `onRenderIBR` function follows a straightforward process, focusing on basic operations like view matrix conversion, rasterization, and CUDA error handling.

- **Scaffold (脚手架)**:
  - The scaffold version of `onRenderIBR` adds several additional steps such as filtering visible Gaussians, computing view parameters, and calculating neural opacity, leading to a more refined and controlled rendering process.
 
- **Original (原始)**:
  - 原始的 `onRenderIBR` 函数遵循简单的流程，主要关注基本操作，如视图矩阵转换、光栅化和 CUDA 错误处理。

- **Scaffold (脚手架)**:
  - 脚手架版本的 `onRenderIBR` 添加了几个额外的步骤，例如过滤可见的高斯点、计算视图参数和计算神经不透明度，从而实现更精细和可控的渲染过程。


![Differences in onRenderIBR](presentation/20.jpg)

## Differences in Submodule CUDA Functions

This section highlights the differences in the CUDA functions within the submodule used by `GaussianView.cpp`. These differences reflect modifications in how Gaussian splats are processed, particularly in the scaffold setup.

本节重点介绍了在 `GaussianView.cpp` 中使用的子模块中的 CUDA 函数的差异。这些差异反映了在脚手架设置中处理高斯斑点的修改。

### 1. Differences in `rasterize_points.h` and `ext.cpp` 

- **Original (原始)**:
  - The original implementation provides basic CUDA functions for rasterizing Gaussian points.

- **Scaffold (脚手架)**:
  - The scaffold version introduces additional parameters and functions, providing more control over the rasterization process, including handling of pre-filtered data and debugging options.
 
- **Original (原始)**:
  - 原始实现提供了用于光栅化高斯点的基本 CUDA 函数。

- **Scaffold (脚手架)**:
  - 脚手架版本引入了额外的参数和函数，为光栅化过程提供了更多的控制，包括预过滤数据的处理和调试选项。


![Differences in Submodule CUDA Functions - 21](presentation/21.jpg)



### 2. Differences in `__init__.py` and `forward.cu` 

- **Original (原始)**:
  - The original code does not include advanced filtering or preprocessing steps for Gaussian points.

- **Scaffold (脚手架)**:
  - The scaffold version adds significant preprocessing and filtering steps in the CUDA code, which are integrated into the Python interface through the `__init__.py` file.

![Differences in Submodule CUDA Functions - 22](presentation/22.jpg)

### 3. Differences in `rasterizer_impl.cu` and Directory Structure 

#### Differences in Submodule CUDA Functions

本节重点介绍了子模块中的 CUDA 函数在原始实现和脚手架实现之间的差异。

- **Original (原始)**:
  - The directory structure and CUDA implementation files are simpler, focusing on core rasterization tasks.
  - 目录结构和 CUDA 实现文件更简单，专注于核心光栅化任务。

- **Scaffold (脚手架)**:
  - The scaffold setup introduces a more complex directory structure with additional files for handling different aspects of Gaussian splat rasterization, such as backward propagation and reweighted sampling.
  - 脚手架设置引入了更复杂的目录结构，并添加了用于处理高斯斑点光栅化不同方面的文件，如反向传播和重加权采样。

![Differences in Submodule CUDA Functions - 23](presentation/23.jpg)

### Summary of Modified or Added `.cu` Files

这是修改或添加的 `.cu` 文件的摘要

- **Modified Files (红色标记)**:
  - Files that have been altered to adapt to the new data structure.
  - 为适应新数据结构而修改的文件。

- **Added Files (紫色标记)**:
  - New files introduced to optimize calculations for rendering, including the addition of new steps in the rendering process.
  - 新引入的文件，用于优化渲染计算，包括在渲染过程中添加新的步骤。

Functions are modified to adapt to the new data structure. Additional functions are added to optimize calculations for rendering, incorporating new steps in the rendering process.

函数经过修改以适应新的数据结构。此外，还添加了其他函数，以优化渲染计算，并在渲染过程中引入了新的步骤。


---

### Additional Resources

For a deeper understanding of CUDA programming, please refer to the included tutorial PDF:

[CUDA Tutorial PDF](reduction-cuda.pdf)

请参阅所附的 CUDA 教程 PDF 以更深入地了解 CUDA 编程：

[CUDA 教程 PDF](reduction-cuda.pdf)

---
## Differences in Spacetime Method

![Differences in Spacetime File Structures](presentation/24.jpg)

This section covers the differences when using the Spacetime method, which allows rendering in 4D by incorporating the dimension of time. This method provides enhanced capabilities for dynamic scenes.

本节介绍了使用 Spacetime 方法时的差异，该方法通过加入时间维度来实现 4D 渲染。该方法为动态场景提供了更强的渲染能力。


### 1. Differences in File Structures 

Recommended checkpoint  structure in the model path location for the spacetime method:

```
<location>
|---point_cloud
|   |---point_cloud.ply
|---cfg_args
|---cameras.json
(|---input.ply)
```


- **Original (原始)**:
  - The original file structure includes basic elements like `point_cloud`, `cameras`, and `cfg_args`, with configurations tailored for static scene rendering.

- **Spacetime (Spacetime)**:
  - The Spacetime version modifies the `cfg_args` and `point_cloud` structures to handle motion data and other dynamic scene elements, enabling the system to process time-variant inputs.

- **Original (原始)**:
  - 原始文件结构包括基本元素，如 `point_cloud`、`cameras` 和 `cfg_args`，这些配置适用于静态场景渲染。

- **Spacetime (Spacetime)**:
  - Spacetime 版本修改了 `cfg_args` 和 `point_cloud` 结构，以处理运动数据和其他动态场景元素，使系统能够处理时间变化的输入。


### 2. Differences in Configuration Arguments

- **Original (原始)**:
  - The original `cfg_args` is configured for a standard 3D scene, with basic options set for static rendering.

- **Spacetime (Spacetime)**:
  - The Spacetime configuration introduces new parameters such as `data_device`, `loader`, and `model_path`, specifically designed to manage 4D data, including motion vectors and temporal resolution adjustments.

- **Original (原始)**:
  - 原始的 `cfg_args` 配置适用于标准 3D 场景，设置了用于静态渲染的基本选项。

- **Spacetime (Spacetime)**:
  - Spacetime 配置引入了新参数，如 `data_device`、`loader` 和 `model_path`，这些参数专门设计用于管理 4D 数据，包括运动向量和时间分辨率调整。

![Differences in Spacetime Configuration](presentation/25.jpg)



### 3. Differences GaussianView.hpp

The Spacetime method utilizes additional characteristics to describe the Gaussians, specifically to understand their movement over time. This requires storing more information compared to other methods.

For more details, please refer to the article: [PDF link](https://arxiv.org/pdf/2312.16812).

Spacetime 方法使用了额外的特性来描述高斯点，特别是为了理解它们随时间的运动。这比其他方法需要存储更多的信息。

有关详细信息，请参阅文章：[PDF 链接](https://arxiv.org/pdf/2312.16812)。

![Differences in Spacetime Configuration](presentation/26.jpg)



### 4. Differences GaussianView.cpp



![Differences in Spacetime Configuration](presentation/27.jpg)


## Differences: onRenderIBR Function

The `onRenderIBR` function differs between the original implementation and the Spacetime implementation. The steps in each version are outlined below:

`onRenderIBR` 函数在原始实现和Spacetime实现之间存在差异。每个版本的步骤如下：

### Original Implementation (原始实现)

1. Render Mode Check (渲染模式检查)
2. Conversion of View and Projection Matrices (视图和投影矩阵的转换)
3. Calculation of Additional View Parameters (附加视图参数的计算)
4. Copy Frame-Dependent Data to GPU (将帧相关数据复制到GPU)
5. Mapping of the OpenGL Resource for Use with CUDA (映射OpenGL资源，以便与CUDA配合使用)
6. Rasterization (光栅化)
7. Unmapping the OpenGL Resource for Use with OpenGL (取消映射OpenGL资源，以便与OpenGL.NET资源一起使用)
8. Copy Image Content to Framebuffer (将图像内容复制到帧缓冲区)
9. CUDA Error Handling (CUDA错误处理)

### Spacetime Implementation (Spacetime实现)

1. Rendering Mode Verification (渲染模式验证)
2. Conversion of View and Projection Matrices (转换视图和投影矩阵)
3. Calculation of Additional View Parameters (计算其他视图参数)
4. Copying Frame-Dependent Data to GPU (将帧相关数据复制到GPU)
5. Mapping OpenGL Resource for Use with CUDA (映射OpenGL资源，以便与CUDA配合使用)
6. Rasterization (栅格化)
7. **Video Time Management (视频时间管理)**
8. **CUDA Rasterizer Call (CUDA光栅调用)**
9. Unmapping OpenGL Resource for Use with OpenGL (取消映射OpenGL资源，以便与OpenGL.NET资源一起使用)
10. Copying Image Content to the Framebuffer (将图像内容复制到帧缓冲区)
11. CUDA Error Management (CUDA错误管理)

![Differences in onRenderIBR Function](presentation/28.jpg)


## Differences in Submodule Folder: GPU-Accelerated Calculations

The submodule folder contains the `.cu` files, which enable fast calculations using the GPU. Below are the differences observed between the Original and Spacetime implementations.

`submodule` 文件夹包含 `.cu` 文件，这些文件使用 GPU 进行快速计算。以下是原始实现和 Spacetime 实现之间的差异。

### Original Implementation (原始实现)

- The folder contains various `.cu` files such as `forward.cu`, `backward.cu`, `rasterizer_impl.cu`, and their corresponding headers like `forward.h`, `backward.h`, and `rasterizer.h`.
- The structure shows the organization of these files, with each `.cu` file responsible for specific GPU-accelerated tasks.

### Spacetime Implementation (Spacetime实现)

- The structure is similar to the original but includes modifications that are likely optimized for Spacetime-specific calculations.
- The `.cu` files and headers are present, but there might be changes in how these files are used or in their content, to handle the 4D rendering tasks.

![Differences in Submodule Folder](presentation/29.jpg)

---

### Additional Resources

For a deeper understanding of CUDA programming, please refer to the included tutorial PDF:

[CUDA Tutorial PDF](reduction-cuda.pdf)

请参阅所附的 CUDA 教程 PDF 以更深入地了解 CUDA 编程：

[CUDA 教程 PDF](reduction-cuda.pdf)

---


## Conclusion
Files to Modify for Scaffold and/or Spacetime.
结论：需要修改的文件用于 Scaffold 和/或 Spacetime



### 1. **main.cpp**: 
   - **Purpose**: For the config argument (`cfg_arg`) if needed.
   - **目的**: 用于配置参数 (`cfg_arg`)（如有需要）。
   - **Method**: Spacetime and/or Scaffold
   - **方法**: Spacetime 和/或 Scaffold

### 2. **GaussianView.cpp**:
   - **Purpose**: Modifications in `loadPLY` and `onRenderIBR`.
   - **目的**: 修改 `loadPLY` 和 `onRenderIBR`。
   - **Method**: Spacetime and Scaffold
   - **方法**: Spacetime 和 Scaffold

### 3. **backward_reweighted.cu**, **forward_reweighted.cu**:
   - **Purpose**: Add new files in `submodules\diff-gaussian-rasterization\cuda_rasterizer\`.
   - **目的**: 在 `submodules\diff-gaussian-rasterization\cuda_rasterizer\` 中添加新文件。
   - **Method**: Scaffold
   - **方法**: Scaffold

### 4. **forward.cu**, **rasterize_impl.cu**:
   - **Purpose**: Modify files in `submodules\diff-gaussian-rasterization\cuda_rasterizer\`.
   - **目的**: 修改 `submodules\diff-gaussian-rasterization\cuda_rasterizer\` 中的文件。
   - **Method**: Spacetime and Scaffold
   - **方法**: Spacetime 和 Scaffold

### 5. **rasterize_point.cu**:
   - **Purpose**: Modify files in `submodules\diff-gaussian-rasterization\`.
   - **目的**: 修改 `submodules\diff-gaussian-rasterization\` 中的文件。
   - **Method**: Scaffold
   - **方法**: Scaffold


![Conclusion](presentation/30.jpg)

