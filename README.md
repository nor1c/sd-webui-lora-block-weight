### # Full Documentation
Go to https://github.com/hako-mikan/sd-webui-lora-block-weight?tab=readme-ov-file#lora-block-weight for full documentation

### # Weights Setting
Enter the identifier and weights.

Unlike the full model, LoRA is divided into 17 blocks, including the encoder. Therefore, enter 17 values.

BASE, IN, OUT, etc. are the blocks equivalent to the full model.

Due to various formats such as Full Model and LyCORIS and SDXL, script currently accept weights for 12, 17, 20, 26, and 61. Generally, even if weights in incompatible formats are inputted, the system will still function. However, any layers not provided will be treated as having a weight of 0.

<br>

**SDXL LoRA (12)**
|1|2|3|4|5|6|7|8|9|10|11|12|
|-|-|-|-|-|-|-|-|-|-|-|-|
|BASE|IN04|IN05|IN07|IN08|MID|OUT0|OUT1|OUT2|OUT3|OUT4|OUT05|

**SDXL - LyCORIS/LoCon (20)**
|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19|20|
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|BASE|IN00|IN01|IN02|IN03|IN04|IN05|IN06|IN07|IN08||MID|OUT00|OUT01|OUT02|OUT03|OUT04|OUT05|OUT06|OUT07|OUT08|


### # SDXL Blocks Detailed

**All documentation below is based on what AI said, so there may be mistakes. 

<img width="1266" height="531" alt="image" src="https://github.com/user-attachments/assets/aa259955-ccfa-489e-93dd-eae61a833afb" />

### # Examples for different purpose of LoRAs

#### # Syntax Format
```
<lora:model_name:overall_weight:model_weight:lbw=blocks>
```

#### # Character LoRA
```
<lora:character_model:1:1:lbw=0,0,0,0,0,0,1,1,0,0,0,0>
```

#### # Style LoRA

Use `lbw=0,0,0,0,0,0,0,0,1,1,1,1` for **OUT** blocks (style, colors, textures).​

Skip **IN**/**MID** (pose, composition, character prefs).

```
<lora:style_model:1:1:lbw=0,0,0,0,0,0,0,0,1,1,1,1>
```

**Personal Test and Proven**: use `<lora:style_model:0.5:lbw=1,1,1,1,1,1,0,1,1,1,1,1>` 

#### # Pose LoRA

Use `lbw=0,1,1,1,1,1,1,1,1,0,0,0` for **IN**/**MID** blocks (pose, position, composition).​

Skip **OUT** blocks to avoid style/character/clothing bleed.

```
<lora:pose_model:1:1:lbw=0,1,1,1,1,1,1,1,1,0,0,0>
```

#### # Avoid Overfiting

Lower overall LoRA weight to 0.5-0.7: `<lora:style_model:0.6:1:lbw=0,0,0,0,0,0,0,0,1,0.8,0.6,0.4>`​

Ramp down later **OUT** blocks (reduce to 0.4-0.6) for style without locking composition/angle, promoting seed diversity.​

Increase CFG (7-9), add prompt variety (angles, "dynamic pose", "varied composition"), enable highres fix (1.5x upscale, 0.3-0.4 denoise).​
