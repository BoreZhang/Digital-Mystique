# Digital Mystique: AI-Powered X-Men Character Skin Transformation

![Mystique Transformation](https://static1.cbrimages.com/wordpress/wp-content/uploads/2018/09/mystique-dark-phoenix-header.jpg)

## 📚 Project Overview

This project implements an innovative digital solution for transforming human skin to match the iconic blue scale appearance of Mystique from X-Men. Traditional makeup for this character requires 6-12 hours of application by a team of artists. Our approach eliminates this process by using advanced AI models in ComfyUI to achieve similar visual effects digitally.

The system works in two phases:
1. **Image Processing**: Converting still images with realistic Mystique skin transformation
2. **Video Processing**: Applying consistent transformation across video sequences

## 🎯 Key Features

- Language-guided image editing using state-of-the-art AI models
- High-quality skin texture transformation with scale patterns
- Precise mask generation for targeted modifications
- Video processing with temporal consistency
- Preservation of identity and facial expressions

## 🔬 Technical Implementation

After extensive experimentation with various methods, I developed an optimal approach combining multiple AI models:

### Attempted Methods
- Initially tried Flux Fill with inpainting approach, but encountered issues with mask alignment
- Explored video models for direct style transfer, but found they required high-quality LoRAs

### Final Solution
- **Image Transformation**: Combined GPT Image and ICEdit with precise masks
  - ICEdit for fast and effective hair and eye color changes
  - Mask generation using human parts segmentation
  - Final composition in Adobe Photoshop

- **Video Transformation**: Implemented Wan 2.1 Fun Control workflow
  - Selected for its ability to perform style transfer without LoRA training
  - Provides good results for moderate motion sequences
  - Used Canny edge detection for motion guidance

## 💻 ComfyUI Workflows

This repository contains two main ComfyUI workflows:

### 1. Mystique_Image.json
A workflow focused on transforming still images into Mystique's appearance using:
- ICEdit for instruction-based image editing
- Human segmentation masks for precise skin area targeting
- Specialized prompts for yellow eyes and red hair transformation

![Image Workflow Preview](https://i.imgur.com/placeholder.jpg)

### 2. Mystique_Video.json
A workflow designed for processing video sequences using:
- Wan 2.1 Fun Control for consistent video generation
- CLIPVision-based processing for improved guidance
- Temporal attention multiplier for smoother results
- Customized prompts for blue scaled skin with metallic texture

![Video Workflow Preview](https://i.imgur.com/placeholder2.jpg)

## 🔧 Installation and Usage

### Prerequisites
- ComfyUI (latest version)
- Required models:
  - ICEdit (for image workflow)
  - normal_lora.safetensors
  - wan2.1_fun_control_1.3B_bf16.safetensors (for video workflow)
  - Wan VAE and CLIP Vision models


### Setup Instructions

1. **Clone this repository**
   ```bash
   git clone https://github.com/BoreZhang/Digital-Mystique.git
   cd Digital-Mystique
   ```

2. **Install ComfyUI**
   - Follow the [official installation instructions](https://github.com/comfyanonymous/ComfyUI)
   - Recommended to use the latest version (v0.3.34+)
   - Install ComfyUI Manager from the [official repository](https://github.com/ltdrdata/ComfyUI-Manager) for easy node management
   - The required custom nodes will be automatically detected and installed when loading the workflows

3. **Download required models**
   
   **For Image Workflow:**
   - ICEdit model
     - Download `normal_lora.safetensors` from [ICEdit GitHub](https://github.com/River-Zhang/ICEdit/releases/download/v0.1.0/normal_lora.safetensors)
     - Place in `ComfyUI/models/loras/`
   - Flux models
     - Download `flux1-fill-dev.safetensors` from [Flux Hub](https://huggingface.co/fluxion/flux1-fill-dev/blob/main/flux1-fill-dev.safetensors)
     - Place in `ComfyUI/models/unet/`
   - Text encoders
     - Download `t5xxl_fp16.safetensors` from [Hugging Face](https://huggingface.co/fluxion/t5xxl-clip/blob/main/t5xxl_fp16.safetensors) 
     - Download `clip_l.safetensors` from [Hugging Face](https://huggingface.co/openai/clip-vit-large-patch14/resolve/main/pytorch_model.bin?download=true) (convert to safetensors format)
     - Place in `ComfyUI/models/clip/`
   - Upscaler
     - Download `4xNomos8kDAT.safetensors` from [Civitai](https://civitai.com/models/124421/4xnomos8k)
     - Place in `ComfyUI/models/upscale_models/`

   **For Video Workflow:**
   - Wan 2.1 Fun Control
     - Download `wan2.1_fun_control_1.3B_bf16.safetensors` from [Comfy-Org/Wan_2.1_ComfyUI_repackaged](https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/diffusion_models/wan2.1_fun_control_1.3B_bf16.safetensors?download=true)
     - Place in `ComfyUI/models/diffusion_models/`
   - Wan VAE
     - Download `wan_2.1_vae.safetensors` from [Hugging Face](https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/vae/wan_2.1_vae.safetensors?download=true)
     - Place in `ComfyUI/models/vae/`
   - Text encoder
     - Download `umt5_xxl_fp8_e4m3fn_scaled.safetensors` from [Hugging Face](https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/text_encoders/umt5_xxl_fp8_e4m3fn_scaled.safetensors?download=true)
     - Place in `ComfyUI/models/text_encoders/`
   - CLIP Vision
     - Download `clip_vision_h.safetensors` from [Hugging Face](https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/clip_vision/clip_vision_h.safetensors?download=true)
     - Place in `ComfyUI/models/clip_vision/`

4. **Import workflows**
   - Launch ComfyUI
   - In the UI, click on "Load" button
   - Import `Mystique_Image.json` or `Mystique_Video.json` workflow file
   - ComfyUI Manager will prompt you to install any missing custom nodes

5. **Prepare test images/videos**
   - For best results, use portrait images with good lighting and clear features
   - For videos, use sequences with moderate motion and consistent lighting
   - Place test media in `ComfyUI/input` directory

### Using the Image Workflow
1. Load an input portrait image
2. Adjust the human segmentation masks if necessary
3. Modify the instruction prompt if desired
4. Run the workflow to generate the transformed image
5. For best results, fine-tune the final output in Photoshop

### Using the Video Workflow
1. Prepare a video with the subject you want to transform
2. Load the video into the workflow
3. Adjust attention parameters for your specific case
4. Run the workflow to generate a transformed video
5. For longer videos, consider processing in segments

## 🔍 Future Improvements

- Integration with newer models like VACE and HunyuanCustom as hardware becomes more accessible
- Development of specialized LoRAs for improved Mystique transformations
- Real-time processing capabilities for live applications

## 📝 Citation

If you use this project in your work, please cite:

```bibtex
@misc{DigitalMystique2025,
  author = {Your Name},
  title = {Digital Mystique: AI-Powered X-Men Character Skin Transformation},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/yourusername/digital-mystique}
}
```

## 🙏 Acknowledgements

We would like to express our sincere gratitude to:

- [Metaphysic.ai](https://metaphysic.ai/) for the project concept and inspiration
- [ComfyUI](https://github.com/comfyanonymous/ComfyUI) team for their excellent AI workflow platform
- [ICEdit](https://github.com/River-Zhang/ICEdit) creators for their innovative single-LoRA image editing technique
- [Wan AI](https://wanai.io/) for developing the Wan 2.1 Fun Control video model
- [HiDream-ai](https://github.com/HiDream-ai/HiDream-e1) team for their instruction-based image editing model
- Contributors to [SD3](https://stability.ai/), [Qwen](https://github.com/QwenLM/Qwen), [umt5-xxl](https://huggingface.co/google/umt5-xxl), [diffusers](https://github.com/huggingface/diffusers) and [HuggingFace](https://huggingface.co/) repositories for their open research
- [GPT-4o Vision](https://openai.com/research/gpt4o) team for advanced image manipulation capabilities
- [FluxAI](https://fluxai.co/) for their Flux models and Fill technology
- Creators of [Human Parts Segmentation](https://github.com/hn3000/comfyui-human-parts) and [RMBG](https://github.com/kijai/ComfyUI-RMBG) tools
- All open-source contributors whose work has made this project possible

This project stands on the shoulders of these giants in the AI and computer vision community, and would not be possible without their commitment to open research and development.

