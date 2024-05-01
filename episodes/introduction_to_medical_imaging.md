---
title: "Introduction to Medical Imaging"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- What are the common different kinds of diagnostic imaging?
- What sorts of computational challenges do they present?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain various common kinds of medical images
- Understand how data for them was created and is organized in these images at a high level

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

Medical imaging uses many technologies including X-rays, computed tomography (CT),
magnetic resonance imaging (MRI),
Ultrasound(US),
positron emmision tomography(PET+) and microscopy. 
Although there are tendancies to use certain technologies, or modalities to
awnser certain clinical questions, many modalities may provide information
of interest in terms of research questions. 
In order to work with digital images at scale we need to use computing.
We will recieve imaging in certain kinds of files. For example an X-ray will usually
be kept at the hospital in DICOM format, but the image itself, the 2D arrays will be in a JPEG inside the DICOM. 
Understanding all the kinds of files we are dealing with and how the images inside them were generated can help us approach them computationally.

Conceptually, we can think of medical images as signals. These signals need various kinds of processing
before they are 'readable' to humans or many of the algorithms we write. 

Historically X-rays were the first common form of medical imaging. The diagram below should help you visualize how they are made. The signal from an X-ray generator crosses the subject. Some tissues attenuate the radiation more than others. The signal is captured by an X-ray detector. 

## X-ray



![schematic of x-ray](fig/x_ray_dia.png)


## Tomography and beyond...

There are several kinds of tomography. This technique produces three dimensional images that allow us to see structures within a subject. When we look at 3-D images we speak of voxels. 
Computed tomographs (CTs) are extremely common, and helpful for many diagnostic questions, but have certain costs in terms of not only time and money but radiation to patients. Therefore many research protocols use existing CTs or non-radiating forms of images. The details of various forms of imaging will be covered in a lecture with slides that accompanies this chapter/episode.  

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Inline instructor notes can help inform instructors of timing challenges
associated with the lessons. They appear in the "Instructor View"

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::




::::::::::::::::::::::::: challenge

## Question : What are some disadvantages to ultrasound in terms of computational analysis?


:::::::::::::::::::::::: solution 

Operator dependant, often with patient data burned in, settings vary, positions vary

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge 

## Challenge 1: Can you think of ways to reduce these problems?

How would optimize research involving biceps brachii ultrasounds?



:::::::::::::::::::::::: solution 

## Solution
 
Possible solutions include:
- reviewing metadata on existing images so it can be matched by machine type
- training technicians to use standardized settings only.
- creating a machine set only on one power setting etc. 
- OCR to get rid off burned in text
- image harmonization/normalization algorithms 



:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::



::::::::::::::::::::::::::::::::::::: callout

There is less standardization around file formats of certain kinds of imaging.

While typical radiological images have settled in how they are recorded in the DICOM
even more novel sequences e.g. arterial spin labbeled sequences are not standardized in terms of how they are recoded in
this file format. Pathology images are not neccesarily stored in DICOM at all, and there is
some controversy as to what file format will come to dominate this important form of medical
imaging. 

::::::::::::::::::::::::::::::::::::::::::::::::


## Visualization

There are increasing ways images can be visualized for human interpreters. 
Optimizing visualization for humans and information for computer vision are
very different processes. 

::::::::::::::::::::::::::::::::::::: keypoints 

- Every type and sequence of imaging gives you different and more information 
- Computationally images have arrays but arrays are surrounded by other data
- Research should be designed with human limitations in mind
- We can expect more imaging modalities in the future


::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
