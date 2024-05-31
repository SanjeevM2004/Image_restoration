# Image Restoration

This repository contains code and resources for image restoration tasks, specifically focused on denoising and image conversion. The provided Jupyter notebook demonstrates the necessary steps to train a denoising model and perform image conversions.

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
  - [Training the Model](#training-the-model)
  - [Image Conversion](#image-conversion)
- [Contributing](#contributing)
- [License](#license)

## Installation
Before using the code, clone the repository and install the required dependencies.

```bash
git clone https://github.com/yourusername/Image-restoration.git
cd Image-restoration
pip install -r requirements.txt
```
## Usage
### Training the Model

To train the denoising model, navigate to the project directory and run the training script with the appropriate configuration file.

```bash
python main_train_psnr.py --opt options/swinir/train_swinir_denoising_color.json
```
## Using pretrained models
For training or testing on a few images you can refer to my colab notebook `CS6777_Seminar_training.ipynb`

## Image Conversion
The notebook includes functions to convert images from various formats (JPEG, JPG, TIF, PNG) to PNG format and to generate Low-Resolution (LR) images from High-Resolution (HR) images.

### Convert Images to PNG
The `convert_to_png` function converts images in a specified input folder to PNG format and saves them in the specified output folder.

```python
from PIL import Image
import os
import shutil

def convert_to_png(input_folder, output_folder):
    # Get a list of all files in the input folder
    files = os.listdir(input_folder)
    
    # Loop through each file
    for file in files:
        # Check if the file is a JPEG, JPG, TIF, or PNG image
        if file.lower().endswith('.jpg') or file.lower().endswith('.tif') or file.lower().endswith('.jpeg') or file.lower().endswith('.png'):
            # Open the image
            with Image.open(os.path.join(input_folder, file)) as img:
                # Construct the output file path
                output_file = os.path.splitext(file)[0] + '.png'
                output_path = os.path.join(output_folder, output_file)
                
                # Convert and save the image
                img.save(output_path, 'PNG')
                print(f"Converted {file} to PNG format.")
        else:
            # If it's not a supported format, just copy the file to the new folder
            shutil.copy(os.path.join(input_folder, file), os.path.join(output_folder, file))
            print(f"Copied {file} as is.")

# Example usage
input_folder = '/path/to/input/folder'
output_folder = '/path/to/output/folder'
convert_to_png(input_folder, output_folder)
```

### Generate LR Images
The `generate_lr_image` function generates Low-Resolution images from High-Resolution images by resizing them with a specified scale factor.

```python
from PIL import Image
import os

def generate_lr_image(hr_image_path, output_folder, scale_factor):
    hr_image = Image.open(hr_image_path)
    lr_image = hr_image.resize((hr_image.width // scale_factor, hr_image.height // scale_factor), Image.BICUBIC)
    lr_image_path = os.path.join(output_folder, os.path.basename(hr_image_path))
    lr_image.save(lr_image_path)
    print(f"Saved LR image: {lr_image_path}")

# Example usage
hr_image_path = '/path/to/high/resolution/image.png'
output_folder = '/path/to/output/folder'
scale_factor = 4
generate_lr_image(hr_image_path, output_folder, scale_factor)
```
## Contributing
Contributions are welcome! Please fork the repository and submit a pull request for any improvements or bug fixes.
