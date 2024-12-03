---
title: "Working with MRI"
teaching: 60
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions 

- What kinds of MRI are there?
- How are MRI data represented digitally?
- How should I organize and structure files for neuroimaging MRI data?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Show common kinds of MRI imaging used in research
- Show the most common file formats for MRI
- Introduce MRI coordinate systems
- Load an MRI scan into Python and explain how the data is stored
- View and manipulate image data
- Explain what BIDS is
- Explain advantages of working with Nifti and BIDS
- Show a method to convert from DICOM to BIDS/NIfTI

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction


When some programmers open their IDE, they type `pwd` because this command allows you to see what directory you are in, and then often `git branch` to see which branch they are on. When many clinicians open an MRI they look at the anatomy, but also figure out which sequence and type of MRI they are looking at. As a digital researcher orienting yourself in terms of MRIs and related computation is also important. In terms of computation MRIs, particularly neuro MRIs, have some quircks that make it worthwhile to think of them as a seperate part of the universe than other medical imaging. Three critical differences are image orientation in terms of anatomy, typical file types and typical packages used to process MRIs. In this lesson we will cover these three issues. 

## Anatomy


To get a better view of MRI processing explore lessons from Carpentries Incubators; namely:

 1. [Introduction to Working with MRI Data in Python](https://carpentries-incubator.github.io/SDC-BIDS-IntroMRI/)
 2. [Introduction to dMRI](https://carpentries-incubator.github.io/SDC-BIDS-dMRI/)
 3. [Functional Neuroimaging Analysis in Python ](https://carpentries-incubator.github.io/SDC-BIDS-fMRI/)

