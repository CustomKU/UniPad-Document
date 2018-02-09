[English Document](https://github.com/0226daniel/UniPad-Document/blob/master/Unipack)

# 유니팩 구조

이 문서는 [UniPad]의 프로젝트 파일인 **유니팩**에 대해서 다룹니다.

## 유니팩이란?

유니팩은 [UniPad]의 프로젝트 파일입니다. [Ableton Live]에서 사용하는 **.als** 파일과는 별개로, 독자적인 구조를 사용합니다. 유니팩은 압축 형식으로 **.zip** 확장자를 사용합니다.

## 유니팩의 구조

유니팩은 크게 **5가지** 파일으로 이루어져 있습니다.

| 이름			| 형식					| 내용
| :------------ | :-------------------: | :----
| [info]		| :page_with_curl:		| 유니팩의 정보를 담는다.
| [sounds]		| :open_file_folder:	| 사운드 파일을 담는 폴더이다.
| [keySound]	| :page_with_curl:		| 사운드 파일을 버튼에 매핑한다.
| [keyLED]		| :open_file_folder:	| LED 이벤트 파일들을 담는 폴더이다.
| [autoPlay]	| :page_with_curl:		| 자동재생을 위한 녹음 기록을 담는다.

## 주의사항

- 유니팩에 사용되는 **파일**은 사운드 파일을 제외하고 **확장자**가 없습니다.
- 파일 포멧은 **UTF-8** (BOM x) 형식을 사용합니다.
- **대소문자**를 문서와 일치시켜주세요.
- x축은 **세로**, y축은 **가로**로 수학적인 좌표계와 반대입니다.
- 시간의 단위는 모두 **ms** 입니다.

## 1. info :page_with_curl:

제목, 제작자, 버튼개수, 체인개수 등 팩에 대한 기본 정보를 정의합니다.

### info의 구조

| 명령어			| 형식		| 내용			|
| ------------- | --------- | ------------- |
| title			| 문자열		| 팩의 제목		|
| producerName	| 문자열		| 팩의 제작자		|
| buttonX		| 자연수		| 세로 버튼 개수	|
| buttonY		| 자연수		| 가로 버튼 개수	|
| chain			| 자연수		| 체인 개수		|
| squareButton	| true/false| 버튼의 유형		|

### 예시

```
title=Alan Walker - Faded
producerName=Abc, Bcd
buttonX=8
buttonY=8
chain=5
squareButton=true
```

### 주의사항

- 이전 형식에 있었던 **landscape** 명령어는 제거되었습니다.

## 2. sounds :open_file_folder:

각 버튼에 사용될 **샘플링된 음악 파일**들을 담습니다.

### 주의사항

- **.wav** 확장자를 사용해야 합니다.
**PCM 16bit** 코덱을 권장하며, [GoldWave]로 변환할 수 있습니다.
- 사운드파일의 이름은 **영어**, **숫자**만 포함해야 하며, **한글이나 공백문자**는 포함될 수 없습니다.
- 사운드는 보통 **6초**까지 지원합니다.

> 가능한 예시
> ```
> 1_01.wav
> drum1.wav
> ```
> 불가능한 예시
> ```
> 1 01.wav
> 드럼1.wav
> sound.mp3
> ```

## 3. keySound  :page_with_curl:

:open_file_folder:sounds에 있는 사운드 파일들을 각 버튼에 매핑합니다.

### keySound의 구조

| 체인	| x	| y	| 파일명		| 반복횟수	|
| ----- | - | - | --------- | --------- |
| 1		| 5	| 6	| 1_01.wav	|			|
| 3		| 8	| 8	| drum1.wav	|			|
| 4		| 1	| 1	| loop.wav	| 0			|

- 반복횟수는 **생략이 가능**하며 기본값은 1입니다.
- 반복횟수가 **0**일 때는 버튼을 **누르고 있는 동안**에 소리가 **반복 재생**됩니다.

### 주의사항

- 확장자 **.wav**까지 작성해야 합니다.
- 다중매핑은 중복된 좌표에 순서대로 표기합니다.
```
1 5 7 13_1.wav
1 5 7 13_2.wav
```

## 4. keyLED :open_file_folder:

LED 이벤트 정보를 담는 파일들을 모아두는 폴더입니다.

### LED 이벤트 파일의 **이름** 구조

| 체인	| x	| y	| 반복횟수	| 순서문자	|
| ----- | - | - | --------- | --------- |
| 1		| 5	| 6	| 			|			|
| 3		| 8	| 8	| 0			|			|
| 4		| 1	| 1	| 1			| a			|
| 4		| 1	| 1	| 1			| 01		|

### 주의사항

- 반복횟수는 **생략이 가능**하며 기본값은 1입니다.
- 반복횟수가 **0**일 때는 버튼을 **누르고 있는 동안**에 LED가 **반복 재생**됩니다.
- 순서문자는 **다중매핑**을 위함이며, **순서문자**를 사용하기 위해서는 **반복횟수**를 생략할 수 없습니다.
- 순서문자는 **영문자**나 **자릿수가 같은 숫자**를 사용합니다.
> 가능한 예시
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
> 불가능한 예시
> ```
> 1 7 3 1 1
> 1 7 3 1 2
> ...
> 1 7 3 1 9
> 1 7 3 1 10
> ```

### LED 이벤트 **파일**의 구조

-----

| on	| x	| y	| 색코드		| 벨로시티	|
| ----- | - | - | --------- | --------- |
| on	| 3	| 4	| FFA726	|			|
| on	| 3	| 4	| 2196F3	| 45		|
| on	| 3	| 4	| auto		| 29		|
| o		| 3	| 4	| FFA726	|			|
| o		| 3	| 4	| 2196F3	| 45		|
| o		| 3	| 4	| a			| 29		|

LED를 (x, y) 좌표에 **색코드** 색상으로 켜고 런치패드에 **벨로시티**를 보냅니다.
**색코드**가 **auto**인 경우에는 자동으로 **색코드**를 정해줍니다.
런치패드 S, mini 기종은 비슷한 색상을 표현해줍니다.

-----

| off	| x	| y	|
| ----- | - | - |
| off	| 3	| 4	|
| f		| 3	| 4	|

LED를 끕니다.

-----

| delay	| ms	|
| ----- | ----- |
| delay	| 120	|
| d		| 120	|

다음 이벤트까지 **지연시간**을 둡니다.

-----

### 주의사항

- 벨로시티 값은 **런치패드와의 통신**을 지원하기 위해서 입니다.
- 색코드 대신 **auto** 모드를 권장합니다.
- 다른 곳에서 켜진 **LED**는 그 파일에서만 끌 수 있습니다.
- 만약 다른 파일에서 끄고싶다면 해당 좌표의 **LED**를 덮어씌우고 **off** 명령어로 끄면 됩니다.
> :page_with_curl: 1 1 1
> ```
> o 1 1 a 45
> ```
> :page_with_curl: 1 1 2
> ```
> o 1 1 a 0
> f 1 1
> ```

- 벨로시티 값은 아래 이미지를 참조하세요.
![launchpad color](http://i.imgur.com/Wc4Yh7j.jpg)
출처 : [launchpad pro's forum](http://forum.launchpad-pro.com/viewtopic.php?id=4055)

## 5. autoPlay :page_with_curl:

팩을 자동으로 연주해주기 위한 **버튼 터치 및 체인 변경**에 대한 데이터를 담습니다.
자동재생을 켠 후 일시정지를 하면 **연습모드**로 전환되며 이 또한 자동재생 데이터 기반으로 작동합니다.
[UniPad]의 녹음 기능으로 **autoPlay** 데이터를 **녹음**할 수 있습니다.

### autoPlay의 구조

-----

| chain	| 체인	|
| ----- | ----- |
| chain	| 3		|
| c		| 3		|

체인을 **변경**합니다.

-----

| on	| x	| y	|
| ----- | - | - |
| on	| 6	| 4	|
| o		| 6	| 4	|

좌표의 버튼을 **누릅**니다.

-----

| off	| x	| y	|
| ----- | - | - |
| off	| 6	| 4	|
| f		| 6	| 4	|

좌표의 버튼에서 터치 이벤트를 **해제**합니다.

-----

| touch	| x	| y	|
| ----- | - | - |
| touch	| 6	| 4	|
| t		| 6	| 4	|

on 명령어와 off 명령어를 **동시**에 실행합니다.

-----

| delay	| ms	|
| ----- | ----- |
| delay	| 120	|
| d		| 120	|

다음 이벤트까지 **지연시간**을 둡니다.

-----

### 주의사항

- 20ms 이내의 이벤트들은 연습모드 실행 시 **동시터치**로 인식됩니다.








[UniPad]:https://www.google.co.kr/search?biw=1920&bih=949&tbm=isch&sa=1&ei=L58bWvfxBMOm0ATa84PQBQ&q=unipad&oq=unipad&gs_l=psy-ab.3..0j0i30k1l9.80809.81424.0.81549.6.6.0.0.0.0.125.597.4j2.6.0....0...1c.1.64.psy-ab..0.6.595....0.jiZ36w7dL2A
[Ableton Live]:https://www.google.co.kr/search?q=ableton+live&source=lnms&tbm=isch&sa=X&ved=0ahUKEwj3_tzK_93XAhVDkJQKHcdgCUEQ_AUICigB&biw=1920&bih=949
[GoldWave]:https://www.goldwave.com/
[info]:https://github.com/0226daniel/UniPad-Document/blob/master/Unipack/README_ko.md#1-info-page_with_curl
[sounds]:https://github.com/0226daniel/UniPad-Document/blob/master/Unipack/README_ko.md#2-sounds-open_file_folder
[keySound]:https://github.com/0226daniel/UniPad-Document/blob/master/Unipack/README_ko.md#3-keysound--page_with_curl
[keyLED]:https://github.com/0226daniel/UniPad-Document/blob/master/Unipack/README_ko.md#4-keyled-open_file_folder
[autoPlay]:https://github.com/0226daniel/UniPad-Document/blob/master/Unipack/README_ko.md#5-autoplay-page_with_curl