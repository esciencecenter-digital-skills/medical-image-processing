---
title: "Introduction to Medical Imaging"
teaching: 45
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- What are the common different types of diagnostic imaging?
- What sorts of computational challenges do they present?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain various common types of medical images
- Explain at a high level how images' metadata is created and organized

::::::::::::::::::::::::::::::::::::::::::::::::


Medical imaging uses many technologies including X-rays, computed tomography (CT),
magnetic resonance imaging (MRI),
Ultrasound(US),
positron emmision tomography(PET+) and microscopy. 
Although there are tendancies to use certain technologies, or modalities to
awnser certain clinical questions, many modalities may provide information
of interest in terms of research questions. 
In order to work with digital images at scale we need to use information technology.
We will recieve imaging in certain kinds of files. For example an X-ray will usually
be kept at the hospital in DICOM format, but the image itself, the 2D arrays will be in a JPEG inside the DICOM. 
Understanding all the kinds of files we are dealing with and how the images inside them were generated can help us approach them computationally.

Conceptually, we can think of medical images as signals. These signals need various kinds of processing
before they are 'readable' by humans or by many of the algorithms we write. 

While thinking about how the information from these signals are stored in different file types may seem less exciting than what the "true information" or final diagnosis from the image was, it is neccesarry to understand this to make the best algorithms possible. For example, a lot of hospital images are essentially JPEGs. This has implications in terms of image quality as we manipulate and resize the images. 

Below we will mention a few details about image generation and capture, then present a table about file formats and information. 

## X-rays


Historically, X-rays were the first common form of medical imaging. The diagram below should help you visualize how they are produced. The signal from an X-ray generator crosses the subject. Some tissues attenuate the radiation more than others. The signal is captured by an X-ray detector. 




![schematic of x-ray](fig/x_ray_dia.png)


Modern (at least increasingly in advanced centers for the last decade) X-rays are born digital. No actual "film" is produced, rather a DICOM file which contains arrays in JPEG files. Technically the arrays could have been, and sometimes even are, put in PNG or other file types, but typically JPEGs are used for X-rays. We could use the metaphor of a wrapped present here. The DICOM file contains  metadata around the image data, wrapping it. The image data itself is a bunch of 2-D arrays, but these have been organized to a specific shape- they are "boxed" by JPEG files. JPEG is a container format. There are JPEG files (emphasis on the plural) because almost no X-ray can be interpreted clinically without multiple perspectives. In chest X-rays this means a anteroposterior and a lateral. Of course we can take X-rays from any angle and even do them repeatedly. This allows for flouroscopy. Flouroscopy images will be stored in a DICOM but can be displayed as movies because they are typically cine files. Cine is a file format that lets you store images in sequence with a frame rate.

## Tomography and beyond...

There are several kinds of tomography. This technique produces three dimensional images, made of voxels, that allow us to see structures within a subject. 
Computed tomographs (CTs) are extremely common, and helpful for many diagnostic questions, but have certain costs in terms of not only time and money but radiation to patients. Therefore many research protocols use existing CTs or non-radiating forms of images. The details of various forms of imaging will be covered in a lecture with slides that accompanies this chapter/episode. Below are a few summaries about various ultra-common imaging types. Keep in mind that manufacturers may have specificities in terms of file types not covered here, and there are many possibilities in terms of how images could potentially be stored. He we will discuss what is common to get in terms of files given to researchers.

## CTs and tomosynthesis

CTs and tomosynthetic images are produced with the same technology. The difference is that in a CT the image is based on a 360 degree capture of the signal. You can conceptualize this as a spinning donut with the generator and receptor oposite each other. Tomosynthesis uses a limited angle instead of going all the way around the patient. In both cases the image output is then made by processing this into a 3D array. We see again, similarly to X-ray, what tissues atentuated the radiation, and what tissues let it pass, but we can visualize it in either 3D or "slices," single layer slices of voxels. It is not uncommon to get CTs as DICOM files which contain raw image data i.e. the 3-D array, which is then processed before viewing, or in some cases stored off as other file types.

## Ultrasounds

Ultrasound can produce multiple complex kinds of images. Typically sonographers produce a lot of B-mode images. They use signal, high frequency sound waves, sent and captured from a piezoelectric probe (also known as a transducer) to get two dimensional images. Just as different tissues attentuate radiation differently, different tissues attenuate these waves differently, and this can help us create images after some processing of the signal. These images can be captured in rapid succession over time, so they can be saved as cine files inside DICOMs. On the other hand the sonographer can choose to record only a single 'frame', in which case a 2D array will ultimately be saved. B-mode is far from the only type of ultrasound. M-mode, like the cine files in B-mode, can also capture motion, but puts it into a a single 2D array of one line of the image over time. In the compound image below you can see a B-mode 2-D image and an M-mode made on the line in it.    

![Image from Cafer Zorkum, mD, PhD on wikidoc.org with creative commons lisence](fig/Mitral_Valve_M_Mode.jpg)

## MRIs

MRIs are images made by utilizing some fairly complicated physics in terms of what we can do to protons (abundant in human tissue) with magnets and radiofrequency waves, and how we capture their signal. The actual signal on an anatomical MRI needs to be processed via Fourier transforms and some other computational work before it is recognizable as anatomy, but the final product we are used to looking at is a 3D array. This array will typical be wrapped inside a DICOM file, but we can transform the image, and parts of the metadata, to a variety of file types. These will be covered in more detail in the next lecture.   

## Other image types

PET scans, nuclear medicine images in general and pathology images are also broadly available. Pathology is currently undergoing a revolution of digitalization, and a typical file format has not emerged. Pathology images may be DICOM, but could also be stored as specific kinds of TIFF files or other file tupes. Beyond the more common types of imaging, researchers are actively looking into new forms of imaging. Some add new information to old modalities like contrast enhanced ultrasound. Others are novel in signal such as terahertz imaging which uses a previously 'unused' part of the electomagnetic radiation spectrum.



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

- Every type of imaging e.g. MRI, CT, ultrasound etc. gives you different information 
- Computationally images have arrays but arrays are surrounded by other data
- Research should be designed with human limitations in mind
- We can expect more imaging modalities in the future


::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
