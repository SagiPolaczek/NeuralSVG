# NeuralSVG: An Implicit Representation for Text-to-Vector Generation

<p align="center">
<img src="docs/teaser.jpg" width="800px"/>
</p>

*NeuralSVG generates vector graphics from text prompts with ordered and editable shapes. Our method supports dynamic conditioning, such as background color, which facilitating the generation of multiple color palettes for a single learned representation.*

#### Sagi Polaczek, Yuval Alaluf, Elad Richardson, Yael Vinker, Daniel Cohen-Or  

> Vector graphics are essential in design, providing artists with a versatile medium for creating resolution-independent and highly editable visual content. Recent advancements in vision-language and diffusion models have fueled interest in text-to-vector graphics generation. However, existing approaches often suffer from over-parameterized outputs or treat the layered structure --- a core feature of vector graphics --- as a secondary goal, diminishing their practical use. Recognizing the importance of layered SVG representations, we propose NeuralSVG, an implicit neural representation for generating vector graphics from text prompts. Inspired by Neural Radiance Fields (NeRFs), NeuralSVG encodes the entire scene into the weights of a small MLP network, optimized using Score Distillation Sampling (SDS). To encourage a layered structure in the generated SVG, we introduce a dropout-based regularization technique that strengthens the standalone meaning of each shape. We additionally demonstrate that utilizing a neural representation provides an added benefit of inference-time control, enabling users to dynamically adapt the generated SVG based on user-provided inputs, all with a single learned representation. Through extensive qualitative and quantitative evaluations, we demonstrate that NeuralSVG outperforms existing methods in generating structured and flexible SVG.

<a href="https://arxiv.org/abs/2501.03992"><img src="https://img.shields.io/badge/arXiv-2412.06753-b31b1b.svg"></a>
<a href="https://sagipolaczek.github.io/NeuralSVG/"><img src="https://img.shields.io/static/v1?label=Project&message=Website&color=red" height=20.5></a> 



## 🔥 NEWS

**`2025/08/01`**: See you in **ICCV 2025** 🍻!

**`2025/04/17`**: Our code is released!

**`2025/01/07`**: Paper is out!

## Table of Contents
- [Examples](#examples)
- [Installation](#installation)
- [Usage](#usage)
  - [Tips](#tips)
  - [Color Control](#color-control)
  - [Aspect Ratio Control](#aspect-ratio-control)
  - [Sketches](#sketches)
- [Citation](#citation)


## Examples
Here are some example outputs:

<p align="center">
<img src="docs/examples_generation_1.jpg" width="800px"/>  
<br>
<p align="center">
<img src="docs/examples_dropout_rooster.jpg" width="700px"/>  
<br>
<p align="center">
<img src="docs/examples_dropout_bunny.jpg" width="700px"/>  
<br>
<p align="center">
<img src="docs/examples_dropout_astronaut.jpg" width="700px"/>  
<br>
<p align="center">
<img src="docs/examples_control_color_sydney.jpg" width="700px"/>  
<br>
<p align="center">
<img src="docs/examples_sketches_margarita.jpg" width="700px"/>  
<br>


## Installation

### Step 0 - Create a new conda env
```
conda create -n neuralsvg_env python=3.10 -y
conda activate neuralsvg_env
```

### Step 1 - Install `diffvg`
Please follow their [installation guide](https://github.com/BachiLi/diffvg?tab=readme-ov-file#install). It might get tricky! be patient :-)

### Step 2 - Install requirements
```
pip install -r requirements.txt
```

### Step 3 - Download LoRAs
```
# Make sure you are in the correct directory
cd ./lora_weights/

# Download using HF's CLI
huggingface-cli download SagiPolaczek/SD2.1-NeuralSVG-LoRAs lora_weights_sd21b_bg_color.safetensors lora_weights_sd21b_bg_color_colorful.safetensors lora_weights_sd21b_sketches.safetensors --local-dir .
```


## Usage

### Tips
* Use **simple prompts** - clear scene and few objects.
* **Seed** - Different seeds results with different outcomes. Moreover, since we don't have control on `diffvg`'s randomness, **we can't reproduce runs.**


### Color Control
```
# Sunflower
python scripts/train.py --config_path config_files/run_shaping.yaml --data.text_prompt="minimalist vector art of a sunflower" --model.toggle_color="true" --model.toggle_color_bg_colors="['light-red', 'light-green', 'light-blue', 'gold', 'gray']" --model.lora_weights="./lora_weights/lora_weights_sd21b_bg_color.safetensors" --log.exp_name='neuralsvg_sunflower'

# Baby penguin
python scripts/train.py --config_path config_files/run_shaping.yaml --data.text_prompt="minimalist vector art of a baby penguin" --model.toggle_color="true" --model.toggle_color_bg_colors="['light-red', 'light-green', 'light-blue', 'gold', 'gray']" --model.lora_weights="./lora_weights/lora_weights_sd21b_bg_color.safetensors" --log.exp_name='neuralsvg_baby_penguin'

# Baby bunny sitting on a stack of pancakes
python scripts/train.py --config_path config_files/run_shaping.yaml --data.text_prompt="minimalist vector art of a baby bunny sitting on a stack of pancakes" --model.toggle_color="true" --model.toggle_color_bg_colors="['light-red', 'light-green', 'light-blue', 'gold', 'gray']" --model.lora_weights="./lora_weights/lora_weights_sd21b_bg_color.safetensors" --log.exp_name='neuralsvg_bunny_pancakes'

# Dog
python scripts/train.py --config_path config_files/run_shaping.yaml --data.text_prompt="minimalist vector art of a dog" --model.toggle_color="true" --model.toggle_color_bg_colors="['light-red', 'light-green', 'light-blue', 'gold', 'gray']" --model.lora_weights="./lora_weights/lora_weights_sd21b_bg_color.safetensors" --log.exp_name='neuralsvg_dog'
```

You can enhance the color difference by using a different LoRA with slightly different prompt. For example:


```
# Colorful Sydney Opera House
python scripts/train.py --config_path config_files/run_shaping.yaml --data.text_prompt="minimalist colorful vector art of Sydney Opera House" --model.toggle_color="true" --model.toggle_color_bg_colors="['light-red', 'light-green', 'light-blue', 'gold', 'gray']" --model.lora_weights="./lora_weights/lora_weights_sd21b_bg_color_colorful.safetensors" --log.exp_name='neuralsvg_sydney'

# Colorful Dog with a hat
python scripts/train.py --config_path config_files/run_shaping.yaml --data.text_prompt="minimalist colorful vector art of a cat with a hat" --model.toggle_color="true" --model.toggle_color_bg_colors="['light-red', 'light-green', 'light-blue', 'gold', 'gray']" --model.lora_weights="./lora_weights/lora_weights_sd21b_bg_color_colorful.safetensors" --log.exp_name='neuralsvg_cat_with_hat'
```

### Aspect Ratio Control
```
TBD
```

### Sketches
```
# Rose
python scripts/train.py --config_path config_files/run_sketching.yaml --data.text_prompt="minimal 2d line drawing of a rose. on a white background."

# Flamingo
python scripts/train.py --config_path config_files/run_sketching.yaml --data.text_prompt="minimal 2d line drawing of a flamingo. on a white background."
```


## Inference
A cool use-case for our implicit representation is that we can load the model, post-training, and use it to infer unlimited amount of background colors, hopefully resulting with different coloring.

A detailed script will be published!





## Citation
If you find this code useful for your research, please consider citing us:

```
@misc{polaczek2025neuralsvgimplicitrepresentationtexttovector,
      title={NeuralSVG: An Implicit Representation for Text-to-Vector Generation}, 
      author={Sagi Polaczek and Yuval Alaluf and Elad Richardson and Yael Vinker and Daniel Cohen-Or},
      year={2025},
      eprint={2501.03992},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2501.03992}, 
}
```


## Star History
[![Star History Chart](https://api.star-history.com/svg?repos=sagipolaczek/neuralSVG&type=Date)](https://www.star-history.com/#sagipolaczek/neuralSVG&Date)
