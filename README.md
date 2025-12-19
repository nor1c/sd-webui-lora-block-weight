# LoRA Block Weight
- custom script for [AUTOMATIC1111's stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) 
- When applying Lora, strength can be set block by block.

- [AUTOMATIC1111's stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) 用のスクリプトです
- Loraを適用する際、強さを階層ごとに設定できます

[<img src="https://img.shields.io/badge/lang-Egnlish-red.svg?style=plastic" height="25" />](#overview)
[<img src="https://img.shields.io/badge/言語-日本語-green.svg?style=plastic" height="25" />](#概要)
[<img src="https://img.shields.io/badge/Support-%E2%99%A5-magenta.svg?logo=github&style=plastic" height="25" />](https://github.com/sponsors/hako-mikan)

> [!IMPORTANT]
> If you have an error :`ValueError: could not convert string to float`  
> use new syntax`<lora:"lora name":1:lbw=IN02>`

> [!IMPORTANT]
> **For those using StabilityMatrix**  
> It seems that it automatically switches to API mode. If you do not intend to use the API, please disable the API option in the package settings. If you wish to use the API, please refer to the section below ["Guide for API users"](#Guide-for-API-users).  
> **StabilityMatrixを使用している方へ**  
> 自動的にAPIモードになるようなので、APIを使用しない場合にはパッケージの設定からAPIのオプションを外してください。APIを使用したい場合には下記[APIを通しての利用について](#APIを通しての利用について)を参照してください。 

## Updates/更新情報
### 2025.01.23.2300(JST)
- Support FLUX
- Fluxに対応しました

By specifying `<lora:"lora name":lbw=ALL:stop=10>`, you can disable the effect of LoRA at the specified step. In the case of character or composition LoRA, a sufficient effect is achieved in about 10 steps, and by cutting it off at this point, it is possible to minimize the impact on the style of the painting  
`<lora:"lora name":lbw=ALL:stop=10>`と指定することで指定したstepでLoRAの効果を無くします。キャラクターや構図LoRAの場合には10 step程度で十分な効果があり、ここで切ることで画風への影響を抑えることが可能です。

# Overview
Lora is a powerful tool, but it is sometimes difficult to use and can affect areas that you do not want it to affect. This script allows you to set the weights block-by-block. Using this script, you may be able to get the image you want.

## Usage
Place lora_block_weight.py in the script folder.  
Or you can install from Extentions tab in web-ui. When installing, please restart web-ui.bat.

### Active  
Check this box to activate it.

### Prompt
In the prompt box, enter the Lora you wish to use as usual. Enter the weight or identifier by typing ":" after the strength value. The identifier can be edited in the Weights setting.  
```
<lora:"lora name":1:0,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0>.  
<lora:"lora name":1:0,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0>.  (a1111-sd-webui-locon, etc.)
<lyco:"lora name":1:1:lbw=IN02>  (a1111-sd-webui-lycoris, web-ui 1.5 or later)
<lyco:"lora name":1:1:lbw=1,1,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0>  (a1111-sd-webui-lycoris, web-ui 1.5 or later)
```
For LyCORIS using a1111-sd-webui-lycoris, syntax is different.
`lbw=IN02` is used and follow lycoirs syntax for others such as unet or else.
a1111-sd-webui-lycoris is under under development, so this syntax might be changed. 

Lora strength is in effect and applies to the entire Blocks.  
It is case-sensitive.
For LyCORIS, full-model blobks used,so you need to input 26 weights.
You can use weight for LoRA, in this case, the weight of blocks not in LoRA is set to 0.　　
If the above format is not used, the preset will treat it as a comment line.

### start, stop step
By specifying `<lora:"lora name":lbw=ALL:start=10>`, the effect of LoRA appears from the designated step. By specifying `<lora:"lora name":lbw=ALL:stop=10>`, the effect of LoRA is eliminated at the specified step. In the case of character or composition LoRA, a significant effect is achieved in about 10 steps, and by cutting it off at this point, it is possible to minimize the influence on the style of the painting. By specifying `<lora:"lora name":lbw=ALL:step=5-10>`, LoRA is activated only between steps 5-10."

### Weights Setting
Enter the identifier and weights.
Unlike the full model, Lora is divided into 17 blocks, including the encoder. Therefore, enter 17 values.
BASE, IN, OUT, etc. are the blocks equivalent to the full model.
Due to various formats such as Full Model and LyCORIS and SDXL, script currently accept weights for 12, 17, 20, 26, and 61. Generally, even if weights in incompatible formats are inputted, the system will still function. However, any layers not provided will be treated as having a weight of 0.

LoRA(17)
|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|  
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|  
|BASE|IN01|IN02|IN04|IN05|IN07|IN08|MID|OUT03|OUT04|OUT05|OUT06|OUT07|OUT08|OUT09|OUT10|OUT11|

LyCORIS, etc.  (26)
|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19|20|21|22|23|24|25|26|  
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|BASE|IN00|IN01|IN02|IN03|IN04|IN05|IN06|IN07|IN08|IN09|IN10|IN11|MID|OUT00|OUT01|OUT02|OUT03|OUT04|OUT05|OUT06|OUT07|OUT08|OUT09|OUT10|OUT11|

SDXL LoRA(12)
|1|2|3|4|5|6|7|8|9|10|11|12|
|-|-|-|-|-|-|-|-|-|-|-|-|
|BASE|IN04|IN05|IN07|IN08|MID|OUT0|OUT1|OUT2|OUT3|OUT4|OUT05|

SDXL - LyCORIS/LoCon(20)
|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19|20|
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|BASE|IN00|IN01|IN02|IN03|IN04|IN05|IN06|IN07|IN08||MID|OUT00|OUT01|OUT02|OUT03|OUT04|OUT05|OUT06|OUT07|OUT08|

Flux (61)
|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19|20|21|22|23|24|25|26|27|28|29|30|31|32|33|34|35|36|37|38|39|40|41|42|43|44|45|46|47|48|49|50|51|52|53|54|55|56|57|58|59|60|61
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|CLIP|T5|IN|D00|D01|D02|D03|D04|D05|D06|D07|D08|D09|D10|D11|D12|D13|D14|D15|D16|D17|D18|S00|S01|S02|S03|S04|S05|S06|S07|S08|S09|S10|S11|S12|S13|S14|S15|S16|S17|S18|S19|S20|S21|S22|S23|S24|S25|S26|S27|S28|S29|S30|S31|S32|S33|S34|S35|S36|S37|OUT|

### Special Values (Random)
Basically, a numerical value must be entered to work correctly, but by entering `R` and `U`, a random value will be entered.  
R : Numerical value with 3 decimal places from 0~1
U : 3 decimal places from -1.5 to 1.5

For example, if `ROUT:1,1,1,1,1,1,1,1,R,R,R,R,R,R,R,R,R`  
Only the OUT blocks is randomized.
The randomized values will be displayed on the command prompt screen when the image is generated.

### Special Values (Dynamic)
The special value `X` may also be included to use a dynamic weight specified in the LoRA syntax. This is activated by including an additional weight value after the specified `Original Weight`.

For example, if `ROUT:X,1,1,1,1,1,1,1,1,1,1,1,X,X,X,X,X` and you had a prompt containing `<lora:my_lore:0.5:ROUT:0.7>`. The `X` weights in ROUT would be replaced with `0.7` at runtime.

> NOTE: If you select an `Original Weight` tag that has a dynamic weight (`X`) and you do not specify a value in the LoRA syntax, it will default to `1`.

### Save Presets

The "Save Presets" button saves the text in the current text box. It is better to use a text editor, so use the "Open TextEditor" button to open a text editor, edit the text, and reload it.  
The text box above the Weights setting is a list of currently available identifiers, useful for copying and pasting into an XY plot. 17 identifiers are required to appear in the list.

### Fun Usage
Used in conjunction with the XY plot, it is possible to examine the impact of each level of the hierarchy.  
![xy_grid-0017-4285963917](https://user-images.githubusercontent.com/122196982/215341315-493ce5f9-1d6e-4990-a38c-6937e78c6b46.jpg)

The setting values are as follows.  
```
NOT:0,0,0,0,0,0,0,0,0,0,0,0,0,0,0  
ALL:1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1  
INS:1,1,1,1,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0  
IND:1,0,0,0,1,1,1,1,0,0,0,0,0,0,0,0,0,0  
INALL:1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0,0  
MIDD:1,0,0,0,1,1,1,1,1,1,1,1,1,0,0,0,0,0  
OUTD:1,0,0,0,0,0,0,0,0,1,1,1,1,1,0,0,0,0  
OUTS:1,0,0,0,0,0,0,0,0,0,0,0,0,1,1,1,1,1,1,1  
OUTALL:1,0,0,0,0,0,0,0,0,0,1,1,1,1,1,1,1,1,1,1 
```
## XYZ Plotting Function
The optimal value can be searched by changing the value of each layer individually.
### Usage 
Check "Active" to activate the function. If Script (such as XYZ plot in Automatic1111) is enabled, it will take precedence.
Hires. fix is not supported. batch size is fixed to 1. batch count should be set to 1.  
Enter XYZ as the identifier of the LoRA that you want to change. It will work even if you do not enter a value corresponding to XYZ in the preset. If a value corresponding to XYZ is entered, that value will be used as the initial value.  
Inputting ZYX, inverted value will be automatically inputted.
This feature enables to match weights of two LoRAs.  
Inputing XYZ for LoRA1 and ZYX for LoRA2, you get,  
```
LoRA1 1,0,0,0,1,1,1,1,1,1,1,1,0,0,0,0,0  
LoRA2 0,1,1,1,0,0,0,0,0,0,0,0,1,1,1,1,1    
```
### Axis type
#### values
Sets the weight of the hierarchy to be changed. Enter the values separated by commas. 0,0.25,0.5,0.75,1", etc.

#### Block ID
If a block ID is entered, only that block will change to the value specified by value. As with the other types, use commas to separate them. Multiple blocks can be changed at the same time by separating them with a space or hyphen. The initial NOT will invert the change, so NOT IN09-OUT02 will change all blocks except IN09-OUT02.

#### seed
Seed changes, and is intended to be specified on the Z-axis.

#### Original Weights
Specify the initial value to change the weight of each block. If Original Weight is enabled, the value entered for XYZ is ignored.

### Input example
X : value, value : 1,0.25,0.5,0.75,1  
Y : Block ID, value : BASE,IN01-IN08,IN05-OUT05,OUT03-OUT11,NOT OUT03-OUT11  
Z : Original Weights, Value : NONE,ALL0.5,ALL  

In this case, an XY plot is created corresponding to the initial values NONE,ALL0.5,ALL.
If you select Seed for Z and enter -1,-1,-1, the XY plot will be created 3 times with different seeds.

### Original Weights Combined XY Plot
If both X and Y are set to Original Weights then an XY plot is made by combining the weights. If both X and Y have a weight in the same block then the Y case is set to zero before adding the arrays, this value will be used during the YX case where X's value is then set to zero. The intended usage is without overlapping blocks.

Given these names and values in the "Weights setting":  
```
INS:1,1,1,0,0,0,0,0,0,0,0,0  
MID:1,0,0,0,0,1,0,0,0,0,0,0  
OUTD:1,0,0,0,0,0,1,1,1,0,0,0  
```
With:  
X : Original Weights, value: INS,MID,OUTD  
Y : Original Weights, value: INS,MID,OUTD  
Z : none  

An XY plot is made with 9 elements. The diagonal is the X values: INS,MID,OUTD unchanged. So we have for the first row:
```
INS+INS  = 1,1,1,0,0,0,0,0,0,0,0,0 (Just INS unchanged, first image on the diagonal)
MID+INS  = 1,1,1,0,0,1,0,0,0,0,0,0 (second column of first row)
OUTD+INS = 1,1,1,0,0,0,1,1,1,0,0,0 (third column of first row)
```

Then the next row is INS+MID, MID+MID, OUTD+MID, and so on. Example image [here](https://user-images.githubusercontent.com/55250869/270830887-dff65f45-823a-4dbd-94c5-34d37c84a84f.jpg)

### Effective Block Analyzer
This function check which layers are working well. The effect of the block is visualized and quantified by setting the intensity of the other bocks to 1, decreasing the intensity of the block you want to examine, and taking the difference.  
#### Range
If you enter 0.5, 1, all initial values are set to 1, and only the target block is calculated as 0.5. Normally, 0.5 will make a difference, but some LoRAs may have difficulty making a difference, in which case, set 0.5 to 0 or a negative value.

#### settings
##### diff color
Specify the background color of the diff file.

##### chnage X-Y
Swaps the X and Y axes. By default, Block is assigned to the Y axis.

##### Threshold
Sets the threshold at which a change is recognized when calculating the difference. Basically, the default value is fine, but if you want to detect subtle differences in color, etc., lower the value.

#### Blocks
Enter the blocks to be examined, using the same format as for XYZ plots.

Here is the English translation in Markdown format:

Here's the English translation, preserving the Markdown format:

### Elemental
Please refer to [Elemental Merge](https://github.com/hako-mikan/sd-webui-supermerger/blob/main/elemental_ja.md) for details.

#### Usage
In the Elemental accordion, set identifiers similar to block specification. Use `lbwe=ATTON` after `lbwe=`. `lbwe=` works without `lbw=`.
`lbwe=` is processed after `lbw=`, so `lbwe=` processing has priority.
```
<lora:"lora name":1:lbw=MIDD:lbwe=ATTNON>
ATTNON:IN05-OUT04:attn:0.8
```

Format is:  
`"identifier":"block specification":"element specification":"weight"`  

Elements are matched partially. `attn1` matches only `attn1`, `attn` matches `attn1` and `attn2`. Multiple layers and elements can be specified by separating with spaces.

Turn on print change to display triggered elements in command prompt.

`ALL0:::0`  
Sets weight of all elements to zero.  

`IN1:IN00-IN11::1`  
Sets all IN elements to 1.  

`ATTNON::attn:1`  
Sets attn to 1 for all layers.

For details of each element like `attn`, please refer to [Elemental Merge](https://github.com/hako-mikan/sd-webui-supermerger/blob/main/elemental_ja.md).

In the input field, each directive is separated by a blank line.  
You can use multiple directives, separated by either a comma or a new line.

```
ATTNDEEPON:IN05-OUT05:attn:1,IN05-OUT05:attn:12

ATTNDEEPOFF:IN05-OUT05:attn:0
BASE:attn:0.5

PROJDEEPOFF:IN05-OUT05:proj:0

XYZ:::1
```

### Guide for API users
#### Regular Usage
By default, Active is checked in the initial settings, so you can use it simply by installing it. You can use it by entering the format as instructed in the prompt. If executed, the phrase "LoRA Block Weight" will appear on the command prompt screen. If for some reason Active is not enabled, you can make it active by entering a value in the API for `"alwayson_scripts"`. 
When you enable API mode and use the UI, two extensions will appear. Please use the one on the bottom.
The default presets can be used for presets. If you want to use your own presets, you can either edit the preset file or use the following format for the data passed to the API. 

The code that can be used when passing to the API in json format is as follows. The presets you enter here will become available. If you want to use multiple presets, please separate them with `\n`.

```json
"prompt": "myprompt, <lora:mylora:1:MYSETS>",
"alwayson_scripts": {
    "LoRA Block Weight": {
        "args": ["MYSETS:1,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0\nYOURSETS:0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,1", true, 1 ,"","","","","","","","","","","","","",""]
    }
}
```
#### XYZ Plot
Please use the format below. Please delete `"alwayson_scripts"` as it will cause an error.

```
"prompt": "myprompt, <lora:mylora:1:XYZ>",
"script_name":"LoRA Block Weight",
"script_args": ["XYZ:1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1", true, 1 ,"seed","-1,-1","","","","","","","","","","","",""]
```
In this case, the six following `True,1` correspond to `xtype,xvalues,ytype,yvalues,ztype,zvalues`. It will be ignored if left blank. Please follow the instructions in the XYZ plot section for entering values. Even numbers should be enclosed in `""`.

The following types are available.

```
"none","Block ID","values","seed","Original Weights","elements"
```
#### Effective Block Analyzer
It can be used by using the following format.

```
"prompt": "myprompt, <lora:mylora:1:XYZ>",
"script_name":"LoRA Block Weight",
"script_args": ["XYZ:1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1", true, 2 ,"","","","","","","0,1","17ALL",1,"white",20,true,"",""]
```
For `"0,1"`, specify the weight. If you specify `"17ALL"`, it will examine all the layers of the normal LoRA. If you want to specify individually, please write like `"BASE,IN00,IN01,IN02"`. Specify whether to reverse XY for `True` in the `"1"` for the number of times you want to check (if it is 2 or more, multiple seeds will be set), and `white` for the background color.

#### Make Weights
In "make weights," you can create a weight list from a slider. When you press the "add to preset" button, the weight specified by the identifier is added to the end of the preset. If a preset with the same name already exists, it will be overwritten. The "add to preset and save" button allows you to save the preset simultaneously.
![makeweights](https://github.com/hako-mikan/sd-webui-lora-block-weight/assets/122196982/9f0f3c1f-d824-45a6-926d-e1b431d5ef61)
