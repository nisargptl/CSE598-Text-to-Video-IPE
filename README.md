# CogVideo Fine-Tuning on a Supercomputer (Single Node Setup)  

This guide provides instructions for setting up and fine-tuning the CogVideo model on a single node of a supercomputer environment using multiple GPUs.  

## Prerequisites  

### 0. Access resources
To get resources to be used interactively on SOL, you can use the following command(This is to get one a30 for 4 hours). For more details and available options, please see the guides here: https://asurc.atlassian.net/wiki/spaces/RC/pages/1908998178/Sol+Hardware+-+How+to+Request
  ```bash
  interactive -N 1 -G a30:1 -t 0-4
  ```

### 1. Clone the Repository  

Clone the `CogVideo` repository to get started. "Since the related code has not been merged into the diffusers release, you need to base your fine-tuning on the diffusers branch." - From CogVideo

    ```bash  
    git clone https://github.com/THUDM/CogVideo.git  
    ```  

### 2. Install Dependencies  

It’s recommended to use a Conda environment to manage dependencies. You can create a new environment and install the necessary packages with:  

    ```bash  
    module load mamba/latest
    mamba create -n cogvideo-env python=3.10  
    mamba activate cogvideo-env  
    cd CogVideo
    pip install -r requirements.txt  
    cd finetuning
    git clone https://github.com/huggingface/diffusers.git
    cd diffusers
    pip install -e .
    cd ..
    mamba install -c conda-forge accelerate wandb transformers peft torchvision sentencepiece gradio openai moviepy pillow scikit-video
    ```  

### 3. Hugging Face CLI Login  

Authenticate your Hugging Face account, as it's required for some dependencies and dataset access.  

    ```bash  
    huggingface-cli login  
    huggingface-cli download \
    --repo-type dataset Wild-Heart/Disney-VideoGeneration-Dataset \
    --local-dir video-dataset-disney
    ```  

## Launching the Fine-Tuning Process  
Modify the finetune_single_rank.sh file with proper variables for dataset location, and other parameters, and run it using

    ```bash  
    bash finetune_single_rank.sh
    ```  

## Monitoring and Troubleshooting  
1. **Common Issues**:  
   - **Out of Memory (OOM) Errors**: If you encounter OOM errors, reduce the batch size in your training script or increase the GPU memory.  
   - **Installation Errors**: Ensure that all dependencies are correctly installed, especially if you encounter import errors.  

By following these steps, you should be able to set up and fine-tune CogVideo on a single node with multiple GPUs. Adjust parameters based on your specific system setup and available resources.
