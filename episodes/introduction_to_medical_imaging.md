---
title: "Introduction to Medical Imaging"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- What are the common different kinds of diagnostic imaging?
- What computational challenges do they present?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain various common kinds of medical images
- Understand how data was created and is organized in these images at a high level

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

Medical imaging uses many technologies including X-rays, computed tomography (Ct),
magnetic resonance imaging (MRI),
Ultrasound(US),
positron emmision tomography(PET+) and microscopy. 
Although there are tendancies to use certain technologies, or modalities to
awnser certain clinical questions, many modalities may provide information
of interest. 
We will recieve imaging in certain kinds of files. For example an X-ray will usually
be kept at the hospital in DICOM format, but the image itself, the 2D arrays will be a JPEG inside the DICOM. 
Understanding both the kinds of files we are dealing with and how the images inside them were generated can help us approach them computationally.
We can think of medical images as signals. These signals need various kinds of processing
before they are 'readable' to humans or many of the algorithms we write. 

Historically X-rays were the first common form of medical imaging. The diagram below should help you visualize how they are made. The signal from an X-ray generator crosses the subject. Some tissues attenuate the radiation more than others. The signal is captured by an X-ray detector. 

## X-ray


`![schematic of X-ray](fig/x_ray_dia.png){alt='schematic of X-rays'}`

![You belong in The Carpentries!](https://raw.githubusercontent.com/carpentries/logo/master/Badge_Carpentries.svg){alt='Blue Carpentries hex person logo with no text.'}

This is a lesson created via The Carpentries Workbench. It is written in
[Pandoc-flavored Markdown](https://pandoc.org/MANUAL.txt) for static files and
[R Markdown][r-markdown] for dynamic files that can render code into output. 
Please refer to the [Introduction to The Carpentries 
Workbench](https://carpentries.github.io/sandpaper-docs/) for full documentation.

What you need to know is that there are three sections required for a valid
Carpentries lesson:

 1. `questions` are displayed at the beginning of the episode to prime the
    learner for the content.
 2. `objectives` are the learning objectives for an episode displayed with
    the questions.
 3. `keypoints` are displayed at the end of the episode to reinforce the
    objectives.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Inline instructor notes can help inform instructors of timing challenges
associated with the lessons. They appear in the "Instructor View"

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: question 

## Question 1: Can you do it?

````Where is the signal in MRI coming from?```



:::::::::::::::::::::::: solution 




## Awnser
 
```awnser
The signal in MRI comes from the patient him or herself (her hydrogen atoms).
```

:::::::::::::::::::::::::::::::::


## Question 2: What are some disadvantages to ultrasound in terms of computational analysis?

:::::::::::::::::::::::: solution 

``Operator dependant, often with patient data burned in, settings vary, positions vary```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Figures

You can use standard markdown for static figures with the following syntax:

`![optional caption that appears below the figure](figure url){alt='alt text for
accessibility purposes'}`

![You belong in The Carpentries!](https://raw.githubusercontent.com/carpentries/logo/master/Badge_Carpentries.svg){alt='Blue Carpentries hex person logo with no text.'}

::::::::::::::::::::::::::::::::::::: callout

Callout sections can highlight information.

They are sometimes used to emphasise particularly important points
but are also used in some lessons to present "asides": 
content that is not central to the narrative of the lesson,
e.g. by providing the answer to a commonly-asked question.

::::::::::::::::::::::::::::::::::::::::::::::::


## Math

One of our episodes contains $\LaTeX$ equations when describing how to create
dynamic reports with {knitr}, so we now use mathjax to describe this:

`$\alpha = \dfrac{1}{(1 - \beta)^2}$` becomes: $\alpha = \dfrac{1}{(1 - \beta)^2}$

Cool, right?

::::::::::::::::::::::::::::::::::::: keypoints 

- Use `.md` files for episodes when you want static content
- Use `.Rmd` files for episodes when you need to generate output
- Run `sandpaper::check_lesson()` to identify any issues with your lesson
- Run `sandpaper::build_lesson()` to preview your lesson locally

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
