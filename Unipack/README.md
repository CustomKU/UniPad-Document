[Korean Document](https://github.com/0226daniel/UniPad-Document/blob/master/Unipack/README_ko.md)

# UniPack Structures

This document is about **UniPack**, which is a project file of [UniPad].

## What is UniPack?

UniPack is a project file of [Unipad]. **.als** file which is used in [Ableton Live] have different structure. UniPack uses **.zip** format.

## The Structures of UniPack

UniPack largely consists of **5 files**.

| Name			| Form					| Contents
| :------------ | :-------------------: | :----
| [info]		| :page_with_curl:		| Include informations of UniPack.
| [sounds]		| :open_file_folder:	| A folder that includes sound files.
| [keySound]	| :page_with_curl:		| Map sound files on the button.
| [keyLED]		| :open_file_folder:	| A folder that includes LED event files.
| [autoPlay]	| :page_with_curl:		| Include autoPlay command.

## Cautions

- **Files** used in UniPack do not have **file extensions** except sound files.
- Recommend **UTF-8** format. (BOM x)
- Match **capitalization** with documents.
- It is opposite with the mathematical coordinate system : X axis is vertical, Y axis is horizontal.
- Units of time are all **ms**.

## 1. info :page_with_curl:

Define basic informations like title, maker, number of buttons, number of chains.

### The Structures of info

| Command		| Format	| Contents
| ------------- | --------- | ----
| title			| String	| Title of pack
| producerName	| String	| Maker of pack
| buttonX		| Integer	| Number of vertical buttons
| buttonY		| Integer	| Number of horizontal buttons
| chain			| Integer	| Number of chains
| squareButton	| true/false| Type of buttons

### Examples

```
title=Alan Walker - Faded
producerName=Abc, Bcd
buttonX=8
buttonY=8
chain=5
squareButton=true
```

### Cautions

- Command **landscape**, which was in last format, was removed.

## 2. sounds :open_file_folder:

Include **Sampled music files** which will be used in each button.

### Cautions

- Must use **.wav** extension.
Encourage **PCM 16bit** codec, and it can be converted by using [GoldWave]
- The name of sound file should only include **English** and **Numbers**. **Blanks** and **Special Characters** cannot be included.
- Sounds are commonly supported up to **6 sec**

> Possible Examples
> ```
> 1_01.wav
> drum1.wav
> ```
> Impossible Examples
> ```
> 1 01.wav
> 드럼1.wav
> sound.mp3
> ```

## 3. keySound  :page_with_curl:

Map sound files in sounds:open_file_folder: on each button.

### The structures of keySound.

| Chain	| x | y | File name	| Repeat Count	|
| ----- | - | - | --------- | -------------	|
| 1		| 5	| 6	| 1_01.wav	|				|
| 3		| 8	| 8	| drum1.wav	|				|
| 4		| 1	| 1	| loop.wav	| 0				|

- The repeat count can be **omitted** and the default value is 1.
- If the repeat count is **0**, sound will  **repeat** while **holding the button**. 

### Cautions

- Must write **.wav** extension.
- Mark multi mapping in repetitive coordinates in sequence.
```
1 5 7 13_1.wav
1 5 7 13_2.wav
```

## 4. keyLED :open_file_folder:

A folder which includes files which contains LED event informations.

##The structures of LED event file **name**

| Chain	| x | y | Repeat Count	| Sequence Letter	|
| ----- | - | - | ------------- | ----------------- |
| 1		| 5	| 6	|				|					|
| 3		| 8	| 8	| 0				|					|
| 4		| 1	| 1	| 1				| a					|
| 4		| 1	| 1	| 1				| 01				|

### Cautions

- The repeat count can be **omitted** and the default value is 1.
- If the repeat count is **0**, the Led will **repeat** while **holding the button**. 
- Character sequences are for **multiple mapping**. And to use **character sequences**, you can't omit **the repeat count**.
- Use Sequence Letter with **English** or **a number of same ciphers**.
> Possible Examples
> ```
> 1 4 2 1 a
> 1 4 2 1 b
> ```
> ```
> 1 7 3 1 01
> 1 7 3 1 02
> ...
> 1 7 3 1 09
> 1 7 3 1 10
> ```
> Impossible Examples
> ```
> 1 7 3 1 1
> 1 7 3 1 2
> ...
> 1 7 3 1 9
> 1 7 3 1 10
> ```

### The structures of LED event **file**

-----

| on	| x	| y	| 색코드		| 벨로시티	|
| ----- | - | - | --------- | --------- |
| on	| 3	| 4	| FFA726	|			|
| on	| 3	| 4	| 2196F3	| 45		|
| on	| 3	| 4	| auto		| 29		|
| on	| *	| 32| auto		| 15		|
| o		| 3	| 4	| FFA726	|			|
| o		| 3	| 4	| 2196F3	| 45		|
| o		| 3	| 4	| a			| 29		|
| o		| *	| 32| a			| 15		|

Turn LED in **color code** on the coordinates and send **velocity** to the launchpad.
If the **color code** is **auto**, it selects **color code** automatically.
Launchpad S, mini show similar color.
If x is \*, set color of circle button. (1~32)

-----

| off	| x	| y	|
| ----- | - | - |
| off	| 3	| 4	|
| f		| 3	| 4	|

Turn off LED.

-----

| delay	| ms	|
| ----- | ----- |
| delay	| 120	|
| d		| 120	|

Make **Delay time** until the next event. 

-----

### Cautions

- Velocity value is to support **communication with launch pad.
- Encourage **auto**mode instead of color code
- **LED** that is turned on the other file only can be turned off in that file.
- If you want to turn off in the other file, overwrite **LED** of the coordinates, and turn off with command **off**.
> :page_with_curl: 1 1 1
> ```
> o 1 1 a 45
> ```
> :page_with_curl: 1 1 2
> ```
> o 1 1 a 0
> f 1 1
> ```

- Refer to next image about the velocity value
![launchpad color](http://i.imgur.com/Wc4Yh7j.jpg)
Origin : [launchpad pro's forum](http://forum.launchpad-pro.com/viewtopic.php?id=4055)

## 5. autoPlay :page_with_curl:

Include data about **button touch and chain change** to play pack automatically.
If you turn on auto play and pause it, it will switches to **Practice mode**, and it also works as an auto play data base.
You can **record** the **autoPlay** data with the recording function of [UniPad].

### The structures of autoPlay

-----

| chain	| num	|
| ----- | ----- |
| chain	| 3		|
| c		| 3		|

**Change** chain.

-----

| on	| x	| y	|
| ----- | - | - |
| on	| 6	| 4	|
| o		| 6	| 4	|

**Press** buttons of coordinates.

-----

| off	| x	| y	|
| ----- | - | - |
| off	| 6	| 4	|
| f		| 6	| 4	|

**Clear** the touch event from the button of the coordinates.

-----

| touch	| x	| y	|
| ----- | - | - |
| touch	| 6	| 4	|
| t		| 6	| 4	|

Run the On command and Off command **simultaneously**.

-----

| delay	| ms	|
| ----- | ----- |
| delay	| 120	|
| d		| 120	|

Make **Delay time** until the next event. 

-----

### Cautions

- Events within 20ms are recognized as as simultaneous touches at practice mode.








[UniPad]:https://www.google.co.kr/search?biw=1920&bih=949&tbm=isch&sa=1&ei=L58bWvfxBMOm0ATa84PQBQ&q=unipad&oq=unipad&gs_l=psy-ab.3..0j0i30k1l9.80809.81424.0.81549.6.6.0.0.0.0.125.597.4j2.6.0....0...1c.1.64.psy-ab..0.6.595....0.jiZ36w7dL2A
[Ableton Live]:https://www.google.co.kr/search?q=ableton+live&source=lnms&tbm=isch&sa=X&ved=0ahUKEwj3_tzK_93XAhVDkJQKHcdgCUEQ_AUICigB&biw=1920&bih=949
[GoldWave]:https://www.goldwave.com/
[info]:https://github.com/0226daniel/UniPad-Document/blob/master/Unipack/README_en.md#1-info-page_with_curl
[sounds]:https://github.com/0226daniel/UniPad-Document/blob/master/Unipack/README_en.md#2-sounds-open_file_folder
[keySound]:https://github.com/0226daniel/UniPad-Document/blob/master/Unipack/README_en.md#3-keysound--page_with_curl
[keyLED]:https://github.com/0226daniel/UniPad-Document/blob/master/Unipack/README_en.md#4-keyled-open_file_folder
[autoPlay]:https://github.com/0226daniel/UniPad-Document/blob/master/Unipack/README_en.md#5-autoplay-page_with_curl