---
title: Setup
---

Setup instructions live in this document. 

## Data Sets

<!--
FIXME: place any data you want learners to use in `episodes/data` and then use
       a relative link ( [data zip file](data/lesson-data.zip) ) to provide a
       link to it, replacing the example.com link.
-->
Downloading the data will be done with the requests library as part of each lesson.

## Software Setup

::::::::::::::::::::::::::::::::::::::: discussion

### Details

Setup for different systems can be done slightly differently.
We reccomend using conda or mamba, but some of you, especially Linux
users may find it more convenient to build the environments by hand.

There are two required environments you will have to be able to use.
One is [image_libraries](https://github.com/esciencecenter-digital-skills/image-processing/blob/main/environment.yml) and the other is [GIULIAMUSTADD? mrilanding ](https://github.com/brainspinner/cvasl/blob/main/environment.yml). You can copy the yaml files and build the environments with conda or mamba.
The GIULIAMUSTADD mrilanding environment is used in all lessons except the one on preparing images for ML. 

:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: spoiler

### Windows

Copy one yaml file at a time and immediately rename the copied to whatever you find memorable. Otherwise you will end up overwriting the files (both begin as environment.yml). Use conda or mamba and run:

```bash
mamba env create -f whatever_you_named_environment.yml
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

