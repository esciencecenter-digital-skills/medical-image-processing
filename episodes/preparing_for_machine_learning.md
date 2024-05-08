---
title: "Preparing Images for Machine Learning"
teaching: 60
exercises: 3
---

:::::::::::::::::::::::::::::::::::::: questions 

- What basic steps are involved in preparing images for machine learning?
- How can I augment my data? 
- How should I handle data from different sources or acquired under different conditions?
- How can I craft features for machine learning through radiomics or volumetrics?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain basic steps involved in preparing images for machine learning
- Demonstrate data augmentation with affines
- Explain pitfalls of data augmentation
- Explain derived features including radiomics and other pipeline derived features
- Review techniques for image harmonization at the image and dataset level

::::::::::::::::::::::::::::::::::::::::::::::::

The images you recieve can be the raw data for machine learning. If you are extremely lucky 
they can be used without much effort, however in many projects the majority of time
spent in building machine learning is spent cleaning up and preparing data. The quality of
the data used will have a profound impact on the quality of the models built. 

One of the first steps towards building a model is simply looking at both some of the data and metadata by hand. 

::::::::::::::::::::::::::::::::::::: challenge 

## Challenge 1: Thinking about metadata

What metadata will you want to look at on almost any radiological dataset?
Hint: Think of metadata that are relevant for brain and thorax imaging. 

:::::::::::::::::::::::: solution 

## Solution
 
You almost always will want to look athe distribution of ages and sex.
These will be relevant metadata whether you are looking at brain MRIs, or
chest X-rays. They will also be relevant in a variety of other cases.

:::::::::::::::::::::::::::::::::



In the real world we often have a question about a specific pathology that is relatively rare,
so we may get a dataset of thousands of normal subjects, and then relatively speaking fewer of
and pathology, and perhaps very few of the pathology we care about.  

::::::::::::::::::::::::::::::::::::: challenge 

## Challenge 1: Can you do it?

What is the way to use the challenges and question?

:::::::::::::::::::::::: solution 

## Best solution
 
Do not peek, try to solve it yourself. The effort will pay off.

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::
