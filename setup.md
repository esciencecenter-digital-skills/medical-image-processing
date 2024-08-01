---
title: Setup
---

## Data Sets

Downloading the data will be done with the requests library as part of each lesson.

## Software Setup

::::::::::::::::::::::::::::::::::::::: discussion

### Details

Setup for different systems can be done slightly differently.
We reccomend using conda or mamba, but some of you, especially Linux
users may find it more convenient to build the environments by hand.

There are two required environments you will have to be able to use.
One is [image_libraries](https://github.com/esciencecenter-digital-skills/med-image-ext/ml_environment.yml) and the other is [GIULIAMUSTADDTO main_landing ](https://github.com/esciencecenter-digital-skills/med-image-ext/main_environment.yml). 
You can get the both 
The GIULIAMUSTADDTO main_landing environment is used in all lessons except the one on preparing images for ML. 
Instructions can be found on the [med-image-ext repo](https://github.com/esciencecenter-digital-skills/med-image-ext/blob/main/README.md), which are meant to be followed after cloning and entering it. 

:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: spoiler

### Windows

Clone the [med-image-ext repo](https://github.com/esciencecenter-digital-skills/med-image-ext). Use conda (or mamba,
and if so substitute conda with mamba) and run:

```bash
conda deactivate # use this if you were in another environment only
conda env create -f main_environment.yml
```
Then

```bash
conda deactivate
conda env create -f ml_environment.yml
```
::::::::::::::::::::::::

:::::::::::::::: spoiler

### MacOS

GIULIAMUSTADD?
Use conda or mamba


::::::::::::::::::::::::


:::::::::::::::: spoiler

### Linux

Use your judgement. If you find conda/mamba an annoyance, build by hand.

::::::::::::::::::::::::

