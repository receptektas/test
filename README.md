# ComfyUI Van Gogh Style Transfer Project

## Table of Contents
1. [Introduction](#introduction)
2. [Project Overview](#project-overview)
3. [Installation](#installation)
4. [Usage](#usage)
5. [Workflow Description](#workflow-description)
6. [Custom Nodes](#custom-nodes)
7. [Custom Modules](#custom-modules)
8. [Experimental Models](#experimental-models)
9. [Experimental Studies](#experimental-studies)
10. [Performance Measurement](#performance-measurement)
11. [Troubleshooting](#troubleshooting)
12. [Contributing](#contributing)
13. [License](#license)

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

## Custom Nodes

The project utilizes several custom nodes to enhance functionality:

- ComfyUI Manager: For easy management of custom nodes and extensions.
- Anyline: Additional image processing capabilities.
- Crystools Save: Enhanced saving options for outputs.
- pythongosssss: Various utility nodes.
- ControlNet Auxiliary Preprocessors: Additional preprocessing options for ControlNet.
- PerformanceMeasurementStartNode and PerformanceMeasurementEndNode: Custom nodes for measuring performance (see [Performance Measurement](#performance-measurement)).

## Custom Modules

- Crystools: Additional tools for image manipulation and workflow management.

## Experimental Models

The project has been tested with various models:

- Checkpoints: `sd_xl_base_1.0.safetensors`, `Van-Gogh-Style-lvngvncnt-v2.ckpt`, `vanGoghDiffusion_v1.ckpt`
- ControlNet Models: `controlnet11Models_canny.safetensors`, `controlnet11Models_softedge.safetensors`, etc.
- LoRA Models: `Van_Gogh_Style.safetensors`, `vincent_van_gogh_xl.safetensors`
- VAE Models: `sdxl_vae.safetensors`, `xlVAEC_f2.safetensors`

Experiment with these models to find the best combination for your specific use case.

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
