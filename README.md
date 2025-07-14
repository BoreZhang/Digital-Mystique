# **Digital Mystique: AI-Powered X-Men Character Skin Transformation**
!(https://github.com/BoreZhang/Digital-Mystique/blob/main/Workflow.png)
## **📚 Project Overview**

This project implements an innovative digital character transformation workflow, designed to resolve the core conflict in generative AI between **identity preservation** and **high-fidelity detail generation**. Using the character Mystique from *X-Men* as a case study, this research addresses the limitations of both time-consuming practical effects and single-model AI approaches.

To overcome this challenge, this project proposes and validates a **synergistic, multi-stage hybrid workflow**. The process first leverages the superior identity preservation of **FLUX.1-Kontext** for a foundational transformation. It then uses precise semantic segmentation to guide **GPT-4o** in generating high-quality skin textures. The outputs are manually composited in **Adobe Photoshop**, and the final static image is animated using **Wan 2.1 Fun Control** for a temporally coherent video. This methodology achieves results superior to any single model, all on consumer-grade hardware.

## **🎯 Key Features**

* **Hybrid AI Architecture**: Combines the strengths of specialized models for an optimal, high-fidelity output.  
* **High-Fidelity Identity Preservation**: Utilizes FLUX.1-Kontext to ensure the actor's core facial features and expressions remain unaltered.  
* **High-Frequency Detail Generation**: Leverages GPT-4o to create photorealistic skin scales and specular highlights.  
* **Precision Mask-Guided Control**: Employs face and body part segmentation for targeted, accurate editing.  
* **Temporally Consistent Video**: Implements a ControlNet-based style transfer method to generate coherent video sequences.  
* **Consumer-Grade Hardware Feasibility**: The entire workflow is designed and validated to run on a GPU with 24GB of VRAM.

## **🔬 Technical Implementation**

The final solution is a four-stage hybrid workflow designed to systematically address the challenges of character transformation.

### **The Final Solution**

1. **Stage 1: Foundational Transformation**  
   * **Tool**: FLUX.1-Kontext  
   * **Goal**: To perform the primary color and feature transformation (skin, hair, eyes) while ensuring complete identity preservation of the actor. The output is a "structurally correct" but detail-smooth base image.  
2. **Stage 2: High-Frequency Detail Infusion**  
   * **Tools**: GPT-4o, jonathandinu/face-parsing, Human Parts Detector  
   * **Goal**: To generate a high-precision mask for all skin areas and guide GPT-4o to inpaint realistic, scaly blue skin texture only within this region. The output is a "detail-rich" image where structure may be slightly altered.  
3. **Stage 3: Synergistic Composition**  
   * **Tool**: Adobe Photoshop  
   * **Goal**: To manually composite the outputs from the first two stages. By layering the detail-rich texture over the structure-perfect base using the generated mask, a final static image is created that combines the best of both models.  
4. **Stage 4: Controlled Video Synthesis**  
   * **Tool**: Wan 2.1 Fun Control  
   * **Goal**: To animate the final static image. The composited image from Stage 3 is used as a style reference, and a Canny edge ControlNet signal is extracted from the source video to ensure motion consistency.

## **💻 ComfyUI Workflows**

This repository contains the core ComfyUI workflow files to implement the automated portions of this pipeline.

### **1\. Mystique\_Image\_Hybrid.json**

This workflow handles the image generation stages (1 and 2\) to produce the two key intermediate assets required for manual composition.

* **FLUX.1-Kontext Nodes**: Generate the structure-perfect base image.  
* **Segmentation & Masking Nodes**: Generate the precise skin mask using Face and Body Parsing nodes.  
* **Note**: This workflow prepares the base image and the mask. The GPT-4o detail layer must be generated externally (via API or other means) and then combined with these assets in Photoshop as described in Stage 3\.

### **2\. Mystique\_Video\_Synthesis.json**

This workflow executes the final video generation task (Stage 4).

* **Inputs**: Requires the source video and the final composited "Mystique" image from Stage 3\.  
* **Wan 2.1 Fun Control**: Serves as the core video generation model.  
* **ControlNet**: Applies a Canny edge detector to the source video to guide the motion and structure of the output.

## **🔧 Installation and Usage**

### **Prerequisites**

* A working installation of [ComfyUI](https://github.com/comfyanonymous/ComfyUI).  
* [ComfyUI Manager](https://github.com/ltdrdata/ComfyUI-Manager) for simplified installation of custom nodes.  
* A CUDA-enabled GPU with at least **24GB of VRAM** is recommended.  
* API access to OpenAI's GPT-4o for the detail-infusion stage.

### **Setup Instructions**

1. **Clone this repository:**  
   Bash  
   git clone https://github.com/BoreZhang/Digital-Mystique.git

   Navigate into the Digital-Mystique directory and place the .json workflow files into your ComfyUI/workflows directory (or load them directly).  
2. Install Custom Nodes:  
   Using the ComfyUI Manager, install the following custom nodes:  
   * human-parser-comfyui-node by CozyMantis (for body segmentation).  
   * A node that supports face parsing (e.g., from the ComfyUI Impact Pack or other community nodes that wrap models like jonathandinu/face-parsing).  
3. Download Required Models:  
   Place the following models into the correct subdirectories within your ComfyUI/models/ folder:  
   * **FLUX.1 Models** (place in ComfyUI/models/flux/):  
     * flux1-schnell.safetensors  
     * flux1-kontext.safetensors  
   * **Text Encoders** (place in ComfyUI/models/clip/):  
     * clip\_g.safetensors  
     * clip\_l.safetensors  
     * t5xxl\_fp16.safetensors  
   * **Segmentation Models** (for human-parser-comfyui-node, place in comfyui/models/schp/):  
     * Download the model based on the LIP dataset as recommended in the node's documentation.  
   * **Video Model** (place in ComfyUI/models/checkpoints/ or a dedicated video model folder):  
     * Wan2.1-Fun-1.3B-Control.safetensors  
4. Restart ComfyUI:  
   Ensure you fully restart ComfyUI after installing new nodes and models for them to be recognized.

### **Execution**

1. **Image Generation**:  
   * Load the Mystique_Image.json workflow in ComfyUI.  
   * Provide your source image.  
   * Run the workflow to generate the structure-perfect base image and the skin mask.  
   * Use these assets along with GPT-4o to create the detail layer, then composite them in Photoshop.  
2. **Video Synthesis**:  
   * Load the Mystique_Video.json workflow.  
   * Input your source video and the final composited image from the previous step.  
   * Run the workflow to generate the final transformed video.

## **📄 Citation**

This project is the practical implementation of the research detailed in the dissertation:

Zhang, B. (2025). *Digital Mystique: A Hybrid Workflow for High-Fidelity Character Transformation*. MSc Dissertation, Bournemouth University.

This work utilizes several key open-source models and frameworks. Please cite the original authors if you build upon this research.

## **🙏 Acknowledgements**

We would like to express our sincere gratitude to the teams and individuals whose work made this project possible:

* **Black Forest Labs (FluxAI)** for their groundbreaking FLUX.1-Kontext model, which was essential for identity preservation.  
* **OpenAI** for the advanced image manipulation and detail generation capabilities of GPT-4o.  
* **Alibaba PAI (Wan AI)** for developing the accessible and effective Wan 2.1 Fun Control video model.  
* **The ComfyUI team** for their excellent and extensible AI workflow platform.  
* **CozyMantis** for creating the invaluable Human Parts Detector node for body segmentation.  
* **Jonathan Dinu** for the face-parsing model, which enabled precise facial segmentation.  
* The creators of **ICEdit** and **HiDream** for their research into instruction-based image editing, which informed our initial comparative analysis.  
* The many contributors to the **Hugging Face** ecosystem and libraries like **Diffusers** for democratizing access to state-of-the-art models.

This project stands on the shoulders of these giants in the AI and computer vision community. Their commitment to open research and development has been fundamental to our success.
