# ComfyUI Van Gogh Style Transfer Project

## Table of Contents
1. [Introduction](#introduction)
2. [Project Overview](#project-overview)
3. [Installation](#installation)
4. [Usage](#usage)
5. [Workflow Description](#workflow-description)
6. [Custom Components](#custom-components)
   - [Custom Nodes](#custom-nodes)
   - [Custom Modules](#custom-modules)
7. [Experimental Models](#experimental-models)
8. [Experimental Studies](#experimental-studies)
9. [Performance Measurement](#performance-measurement)
10. [Troubleshooting](#troubleshooting)
11. [Contributing](#contributing)
12. [License](#license)

## Introduction

This project implements a ComfyUI workflow for transforming input images into Van Gogh style representations using Stable Diffusion and ControlNet. The workflow is designed to be efficient, user-friendly, and capable of handling various input image types.

## Project Overview

The main objective of this project is to create a ComfyUI workflow that:
- Accepts user-uploaded images in common formats (JPEG, PNG)
- Preprocesses the images for compatibility with the style transfer models
- Applies a Van Gogh style transfer using Stable Diffusion and ControlNet
- Outputs a high-quality, stylized version of the input image

## Installation

1. Ensure you have Python 3.8+ installed on your system.
2. Clone the ComfyUI repository:
   ```
   git clone https://github.com/comfyanonymous/ComfyUI.git
   cd ComfyUI
   ```
3. Install the required dependencies:
   ```
   pip install -r requirements.txt
   ```
4. Install additional custom nodes:
   ```
   pip install comfyui-manager anyline crystools pythongosssss
   ```
5. Download the necessary model checkpoints and place them in the `models` directory:
   - Stable Diffusion models: `sd_xl_base_1.0.safetensors`, `Van-Gogh-Style-lvngvncnt-v2.ckpt`, `vanGoghDiffusion_v1.ckpt`
   - ControlNet models: `controlnet11Models_canny.safetensors`, `controlnet11Models_softedge.safetensors`, etc.
   - LoRA models: `Van_Gogh_Style.safetensors`, `vincent_van_gogh_xl.safetensors`
   - VAE models: `sdxl_vae.safetensors`, `xlVAEC_f2.safetensors`

6. Copy the custom node scripts (`PerformanceMeasurementStartNode.py` and `PerformanceMeasurementEndNode.py`) to the `custom_nodes` directory.

## Usage

1. Start the ComfyUI server:
   ```
   python main.py
   ```
2. Open a web browser and navigate to `http://localhost:8188`.
3. Import the provided workflow JSON file into ComfyUI.
4. Upload your input image using the "Load and Upscale (Input)" node.
5. Adjust any parameters as needed (e.g., prompt strength, ControlNet settings).
6. Click "Queue Prompt" to start the style transfer process.
7. The output image will be displayed in the UI and saved to the specified output directory.

## Workflow Description

The workflow consists of several key components:

1. **Image Loading and Preprocessing**: 
   - The "Load and Upscale (Input)" node handles image upload and initial preprocessing.
   - The "Load and Upscale (Original)" node loads a reference Van Gogh image for comparison.

2. **ControlNet Processing**:
   - The CannyEdgePreprocessor node applies edge detection to the input image.
   - The "Load and Apply ControlNet" node applies the ControlNet model to guide the style transfer.

3. **Style Transfer**:
   - The "Load Checkpoints and CLIP Text Encode" node loads the necessary models and encodes the text prompts.
   - The KSampler node performs the actual style transfer using the loaded models and conditioning.

4. **Output and Performance Measurement**:
   - The VAEDecode node converts the latent representation back into an image.
   - The PerformanceMeasurementStartNode and PerformanceMeasurementEndNode measure the performance of the process.
   - The SaveImage node saves the final output.

## Custom Components

This project incorporates a variety of custom components to enhance functionality and optimize the workflow. These components are categorized into Custom Nodes and Custom Modules, each serving specific purposes within the ComfyUI framework.

### Custom Nodes

1. ComfyUI Manager: Facilitates efficient management of custom nodes and extensions.
2. Anyline: Expands image processing capabilities with additional tools.
3. Crystools Save: Provides advanced options for saving output files.
4. pythongosssss: Offers a collection of utility nodes for various tasks.
5. ControlNet Auxiliary Preprocessors: Introduces extra preprocessing options tailored for ControlNet integration.
6. Performance Measurement Nodes:
   - PerformanceMeasurementStartNode
   - PerformanceMeasurementEndNode
   
   These custom nodes, developed specifically for this project, enable precise performance tracking and analysis. They are located in the custom_nodes directory and are essential for optimizing workflow efficiency. For detailed information on their functionality and implementation, please refer to the Performance Measurement section.

### Custom Modules

1. Crystools: Enhances the project with additional tools for sophisticated image manipulation and streamlined workflow management.

By integrating these custom components, our project achieves a high level of flexibility and power, allowing for advanced image processing and style transfer operations within the ComfyUI ecosystem.

## Experimental Models

The project has been tested with various models:

- Checkpoints: `sd_xl_base_1.0.safetensors`, `Van-Gogh-Style-lvngvncnt-v2.ckpt`, `vanGoghDiffusion_v1.ckpt`
- ControlNet Models: `controlnet11Models_canny.safetensors`, `controlnet11Models_softedge.safetensors`, etc.
- LoRA Models: `Van_Gogh_Style.safetensors`, `vincent_van_gogh_xl.safetensors`
- VAE Models: `sdxl_vae.safetensors`, `xlVAEC_f2.safetensors`

5. Download the necessary model checkpoints and place them in the `models` directory:
   - Stable Diffusion model: `Van-Gogh-Style-lvngvncnt-v2.ckpt`
   - ControlNet model: `controlnet11Models_canny.safetensors`

### Model Selection Rationale

After extensive testing and performance analysis, we have determined that the combination of `Van-Gogh-Style-lvngvncnt-v2.ckpt` for the main Stable Diffusion model and `controlnet11Models_canny.safetensors` for ControlNet provides the optimal balance of performance, simplicity, and efficiency for our Van Gogh style transfer task.

#### Stable Diffusion Model

The `Van-Gogh-Style-lvngvncnt-v2.ckpt` model was selected for its specialized training in Van Gogh's artistic style. In our benchmarks, this model consistently produced outputs that closely emulated Van Gogh's distinctive brushstrokes, color palette, and overall aesthetic, while maintaining a good balance between stylization and preservation of the original image's content.

Compared to generic models like `sd_xl_base_1.0.safetensors`, the Van Gogh-specific model required fewer steps and lower computational resources to achieve satisfactory results. The `vanGoghDiffusion_v1.ckpt` model, while also specialized, showed slightly lower performance in terms of output quality and inference speed.

#### ControlNet Model

We opted for the `controlnet11Models_canny.safetensors` ControlNet model due to its superior performance in preserving the structural integrity of input images while allowing for stylistic transformations. The Canny edge detection preprocessing step provides a robust structural guide for the style transfer process, ensuring that the output maintains the key features of the original image.

In our tests, this model outperformed alternatives such as `controlnet11Models_softedge.safetensors` in terms of both output quality and processing speed. The Canny edge detection proved more reliable across a diverse range of input images compared to other preprocessing methods.

### Performance Impact of Custom VAE and LoRA Models

During our experimentation, we observed that the inclusion of custom VAE (Variational Autoencoder) and LoRA (Low-Rank Adaptation) models, such as `sdxl_vae.safetensors`, `xlVAEC_f2.safetensors`, `Van_Gogh_Style.safetensors`, and `vincent_van_gogh_xl.safetensors`, resulted in suboptimal outcomes both in terms of output quality and computational efficiency.

#### VAE Models

The custom VAE models (`sdxl_vae.safetensors` and `xlVAEC_f2.safetensors`) were initially tested to potentially enhance the encoding and decoding processes. However, we observed the following issues:

1. Increased computational overhead: The custom VAE models required additional GPU memory and processing time, increasing the overall latency of the pipeline by approximately 15-20%.

2. Color shift and artifacts: In many cases, the custom VAE models introduced undesirable color shifts and subtle artifacts in the output images, deviating from the authentic Van Gogh color palette and brushstroke patterns.

3. Inconsistent results: The custom VAE models showed varying performance across different input images, leading to unpredictable output quality.

#### LoRA Models

The LoRA models (`Van_Gogh_Style.safetensors` and `vincent_van_gogh_xl.safetensors`) were evaluated for their potential to fine-tune the style transfer process. However, our analysis revealed several drawbacks:

1. Overfitting to specific features: The LoRA models tended to overemphasize certain Van Gogh-style elements, sometimes resulting in outputs that appeared caricatured or exaggerated rather than naturally stylized.

2. Reduced generalization: While performance improved for images similar to the LoRA training data, we observed a significant drop in quality for diverse input images, indicating poor generalization.

3. Computational inefficiency: Incorporating LoRA models into the pipeline increased the number of parameters and computational steps, resulting in a 10-25% increase in processing time, depending on the specific model and input image.

4. Diminishing returns: The marginal improvement in style transfer quality, when present, did not justify the additional complexity and resource requirements introduced by the LoRA models.

In conclusion, our rigorous testing and analysis demonstrated that the lean combination of `Van-Gogh-Style-lvngvncnt-v2.ckpt` and `controlnet11Models_canny.safetensors` provides the most efficient, consistent, and high-quality results for our Van Gogh style transfer task. This setup minimizes computational overhead while maximizing output quality, allowing for a more streamlined and reliable workflow.

## Experimental Studies

### PyQt5 Desktop Application

An experimental desktop application was developed using PyQt5 to provide a user-friendly interface for the ComfyUI workflow. The application aims to:

- Launch a ComfyUI server in the background
- Present a simplified interface for end-users
- Handle all ComfyUI operations seamlessly

The development of this application progressed significantly but was not fully completed before the project conclusion. All relevant documentation and code for this experimental study can be found in the `experimental_studies/` directory.

## Performance Measurement

Two custom nodes were developed to measure the performance of the workflow:

1. **PerformanceMeasurementStartNode**: Initializes performance tracking at the start of the process.
2. **PerformanceMeasurementEndNode**: Calculates and reports performance metrics at the end of the process.

These nodes measure:
- Execution time
- GPU memory usage
- Image similarity metrics (SSIM, Feature Similarity, Perceptual Loss, etc.)

For detailed information on these custom nodes, refer to the `custom_nodes/performance_measurement.md` file.

## Troubleshooting

- **Issue**: CUDA out of memory error
  **Solution**: Reduce the image size or batch size in the workflow

- **Issue**: Missing custom nodes
  **Solution**: Ensure all required custom nodes are installed and placed in the correct directory

- **Issue**: Slow performance
  **Solution**: Check GPU usage, reduce image size, or optimize the workflow by removing unnecessary nodes

## Contributing

Contributions to this project are welcome. Please follow these steps:

1. Fork the repository
2. Create a new branch for your feature
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
