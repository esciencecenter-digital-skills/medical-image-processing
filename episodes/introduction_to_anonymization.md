---
title: "Intro to Anonymization"
teaching: 40
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- What kinds of data make patient's imaging data identifiable?
- How can I make medical image data safe to share?
- How can I strip out some metadata from DICOMs?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Show examples of data that makes patient images is identifiable
- Discuss the concept of anonmyization
- Use pydicom library as an example to handle DICOM metadata

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

Each of us is similar yet unique. This reality makes us identifiable. Metadata e.g. patient name and adressm,are obviously designed to be patient identifying. It is equally important to recognize that in certain contexts this means all kinds of images can also be identified as belonging to a certain person. With the advent of face recognition software and search engines, even images we never thought of as identifiable, like a head CT, MRI [or even PET](https://doi.org/10.1016/j.neuroimage.2022.119357), can be theoretically traced back to a specific patient. 

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Inline instructor notes: if Zenodo is unavailable ...

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## Types of patient identifying data scans

### Metadata

DICOMs contain metadata. This metadata will have all kinds of identifying data which should not be seen by anyone unless neccesary. The easiest way to avoid problems with DICOM metadata is to never have it in the first place. If you have a choice, it may be preferable to recieve simply images and select metadata, as opposed to whole DICOMs. Certainly when sharing data with collaborators there is almost no reason to share DICOMs. 

### Faces in Images

A full CT, MRI or PET of the head can be reconstructed into a face. Therefore many image analysis programs 'deface' these types of images. This is useful for not only avoiding the patient's specific identity but also to some extent their demographic identity (ethnicity and gender). 

### Text on Images

Occasionaly for various reasons techs will burn information strauight onto images. This may knclude anything from diagnosis to demographics to the patient's name. 

### Other parts of images

With only a few pieces of data, we can often put together patient identity.
Sometimes it would actually only take a single piece of data to track down someone's identity if you had access to medical files. For example the serial number or other medical device identifying number may be traceable back to a specific patient. 
In other cases, it would only take slightly more data to figure out who a specific patient was. For example some patients may have jewlery which is relatively unique such as a medic-alert bracelet or necklace with initials or a name. While most routine ambulatory images should come without jewlery, in acute emergencies workers may not have had to time to strip all of this off of patients. The more data points we have on a patient the more easily we can figure out who it is.


![](fig/jewellery_artifact.jpg){alt='jewlery artifact'}

*Case courtesy of Ian Bickle, <a href="https://radiopaedia.org/">Radiopaedia.org</a>. From the case <a href="https://radiopaedia.org/cases/61830">rID: 61830</a>*


Keep in mind that many data leaks are data leaks within a hospital. People see information on patients they are not treating. Sometimes this is done on purpose e.g. people try to figure out the medical information of colleagues or family members. 


:::::::::::::::::::::::::::::::::::::::: keypoints

- Imaging itself without any metadata can be used to identify patients
- There are 

::::::::::::::::::::::::::::::::::::::::::::::::::
