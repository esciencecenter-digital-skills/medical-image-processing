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


When some programmers open their IDE, they type `pwd` because this command allows you to see what directory you are in, and then often `git branch` to see which branch they are on. When many clinicians open an MRI they look at the anatomy, but also figure out which sequence and type of MRI they are looking at. As a digital researcher orienting yourself in terms of MRIs and related computation is also important. In terms of computation MRIs, particularly neuro MRIs, have some quircks that make it worthwhile to think of them as a seperate part of the universe than other medical imaging. Three critical differences between MRIs and other medical imaging are image orientation in terms of anatomy, typical file types and typical packages used for processing. In this lesson we will cover these three issues.



## File formats

From the MRI scanner, MRI images are initially collected and put in the DICOM format but can be converted to other formats to make working with the data easier. Some file formats can be converted to others, for exaple NiFTIs or Analyze files can be converted to MINC, but not conversions can not be made from all file formats in all directions.

### Common MRI File Formats

| Format Name | File Extension | Origin/Group                                  | More info|
| ----------- | -------------- | --------------------------------------------- |-----------
| DICOM       | none or `.dc`    | ACR/NEMA Consortium                           |https://www.dicomstandard.org/  |
| Analyze     | `.img`/`.hdr`      | Analyze Software, Mayo Clinic                 |https://eeg.sourceforge.net/ANALYZE75.pdf|
| NIfTI       | `.nii`  (or `.nii.gz`) | Neuroimaging Informatics Technology Initiative|https://brainder.org/2012/09/23/the-nifti-file-format/|
| MINC        | `.mnc`           | Montreal Neurological Institute               |https://www.mcgill.ca/bic/software/minc|
| NRRD        | `.nrrd`          |                                               |https://teem.sourceforge.net/nrrd/format.html|
| MGH         |`.mgz`  or `.mgh` (or `.mgh.gz`) | Massachusetts General Hospital|https://surfer.nmr.mgh.harvard.edu/fswiki/FsTutorial/MghFormat|



NIfTI is one of the most ubiquitous file formats for storing neuroimaging data.
We can convert DICOM data to NIfTI using [dcm2niix](https://github.com/rordenlab/dcm2niix) software.

We can learn how to run `dcm2niix` by taking a look at its help menu:

```bash
dcm2niix -help
```
One of the advantages of working with `dcm2niix` is that it can be used to create Brain Imaging Data Structure (BIDS) files, since it outputs a NIfTI and a JSON with metadata ready to fit into the BIDS standard. [BIDS](https://bids.neuroimaging.io/) is a widely adopted standard of how data from neuroimaging research can be organized. The organization of data and files is crucial for seamless collaboration across research groups and even between individual researchers. Some pipelines assume your data is organized in BIDS structure, and these are sometimes called [BIDS Apps](https://bids-apps.neuroimaging.io/apps/).

Some of the more popular examples are:

- `fmriprep`
- `freesurfer`
- `micapipe`
- `SPM`
- `MRtrix3_connectome`

We recommend the [BIDS starter-kit website](https://bids-standard.github.io/bids-starter-kit/#) for learning the basics of this standard.
Working with BIDS or and much of the array of tools available for brain MRIs is often facilitated by some familiarity with writing command line. This could present bit of a problem in terms of scientific reproduciblity. One option is to leave bash scripts and good documentation in a repository. Another is to use a pythonic interface to such command line tools. One possibility for this will be discused in the next section.

## Libraries

If you look at python code for research on medical imaging of most of the body, a few libraries, such as SITK, will pop up again and again. The story with MRIs of the brain is entirely different. Most researchers will use libraries like nibabel and related libraries. These libraries are made to read, write, and manipulate neuroimaging file formats efficiently. There is lots of code developed in them for the specific tasks related to brain MRIs. They also tend to integrate better with existing pipelines for brain MRI. nipype is a popular tool which attempts to allow users to harness popular exisitng pipelines. If you are trying to compare outputs with an existing study, it is worth considering using the same pipeline and software version of the pipeline. Then you know differences between your outcomes are not artifacts of the softwares. 


<img src="fig/nipreps-chart.png" alt="Nipreps chart" width="70%;"/>

*Sourced from [https://www.nipreps.org/](https://www.nipreps.org/)*



:::::::::::::::: callout

### Nipreps and Beyond:

- There are many, many packages for medical image analysis
- There are known pre-built pipelines with possibilities in python
- Your pipeline will probably begin with cleaning and preparing data 
- You can mix and match parts of pipelines with NiPype

::::::::::::::


While you may harness sophisticated tools like Freesurfer or FSL to do complex tasks like segmentation and registration in ways taht match exisitng research, you should not avoic checking your data by hand. At some point you should open images and examine them both in terms of the imaging (which could be done with a pre-buit viewer) and the computational aspects like metadata and shape. For these mundane tasks we reccomend nibabel. 

Next, we'll cover some details on working with NIfTI files with Nibabel.

## Reading NIfTI Images

[NiBabel](https://nipy.org/nibabel/) is a Python package for reading and writing neuroimaging data.
To learn more about how NiBabel handles NIfTIs, refer to the [NiBabel documentation on working with NIfTIs](https://nipy.org/nibabel/nifti_images.html), which this episode heavily references.

```python
import nibabel as nib
```

First, use the `load()` function to create a `NiBabel` image object from a NIfTI file.

```python
t2_img = nib.load("data/mri//OBJECT_phantom_T2W_TSE_Cor_14_1.nii")
```

When loading a NIfTI file with `NiBabel`, you get a specialized data object that includes all the information stored in the file. Each piece of information is referred to as an **attribute** in Python's terminology. To view all these attributes, simply type `t2_img.` followed by <kbd>Tab</kbd>.
Today, we'll focus on discussing mainly two attributes (`header` and `affine`) and one method (`get_fdata`). 

### 1. [Header](https://nipy.org/nibabel/nibabel_images.html#the-image-header)

It contains metadata about the image, including image dimensions, data type, and more.

```python
t2_hdr = t2_img.header
print(t2_hdr)
```

```output
<class 'nibabel.nifti1.Nifti1Header'> object, endian='<'
sizeof_hdr      : 348
data_type       : b''
db_name         : b''
extents         : 0
session_error   : 0
regular         : b''
dim_info        : 0
dim             : [  3 432 432  30   1   1   1   1]
intent_p1       : 0.0
intent_p2       : 0.0
intent_p3       : 0.0
intent_code     : none
datatype        : int16
bitpix          : 16
slice_start     : 0
pixdim          : [1.        0.9259259 0.9259259 5.7360578 0.        0.        0.
 0.       ]
vox_offset      : 0.0
scl_slope       : nan
scl_inter       : nan
slice_end       : 29
slice_code      : unknown
xyzt_units      : 2
cal_max         : 0.0
cal_min         : 0.0
slice_duration  : 0.0
toffset         : 0.0
glmax           : 0
glmin           : 0
descrip         : b'Philips Healthcare Ingenia 5.4.1 '
aux_file        : b''
qform_code      : scanner
sform_code      : unknown
quatern_b       : 0.008265011
quatern_c       : 0.7070585
quatern_d       : -0.7070585
qoffset_x       : 180.81993
qoffset_y       : 21.169691
qoffset_z       : 384.01007
srow_x          : [1. 0. 0. 0.]
srow_y          : [0. 1. 0. 0.]
srow_z          : [0. 0. 1. 0.]
intent_name     : b''
magic           : b'n+1'
```

`t2_hdr` is a Python **dictionary**, i.e. a container that hold pairs of objects - keys and values. Let's take a look at all of the keys.

Similar to `t2_img`, in which attributes can be accessed by typing `t2_img.` followed by <kbd>Tab</kbd>, you can do the same with `t2_hdr`.

In particular, we'll be using a **method** belonging to `t2_hdr` that will allow you to view the keys associated with it.

```python
print(t2_hdr.keys())
```

```output
['sizeof_hdr',
 'data_type',
 'db_name',
 'extents',
 'session_error',
 'regular',
 'dim_info',
 'dim',
 'intent_p1',
 'intent_p2',
 'intent_p3',
 'intent_code',
 'datatype',
 'bitpix',
 'slice_start',
 'pixdim',
 'vox_offset',
 'scl_slope',
 'scl_inter',
 'slice_end',
 'slice_code',
 'xyzt_units',
 'cal_max',
 'cal_min',
 'slice_duration',
 'toffset',
 'glmax',
 'glmin',
 'descrip',
 'aux_file',
 'qform_code',
 'sform_code',
 'quatern_b',
 'quatern_c',
 'quatern_d',
 'qoffset_x',
 'qoffset_y',
 'qoffset_z',
 'srow_x',
 'srow_y',
 'srow_z',
 'intent_name',
 'magic']
```

Notice that **methods** require you to include () at the end of them  when you call them whereas **attributes** do not.
The key difference between a method and an attribute is:

- Attributes are *variables* belonging to an object and containing information about their properties or characteristics
- Methods are functions that belong to an object and operate on its attributes. They differ from regular functions by implicitly receiving the object (`self`) as their first argument.

When you type in `t2_img.` followed by <kbd>Tab</kbd>, you may see that attributes are highlighted in orange and methods highlighted in blue. 

The output above is a list of **keys** you can use to access **values** of `t2_hdr`. We can access the value stored by a given key by typing:

```python
print(t2_hdr['<key_name>'])
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge: Extract Values from the NIfTI Header

Extract the 'pixdim' field from the NiFTI header of the loaded image.

:::::::::::::::  solution

## Solution

```python
print(t2_hdr['pixdim'])
```

```output
array([1. , 0.9259259, 0.9259259, 5.7360578, 0. , 0. , 0. , 0. ], dtype=float32)
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### 2. Data

As you've seen above, the header contains useful information that gives us information about the properties (metadata) associated with the MR data we've loaded in. Now we'll move in to loading the actual *image data itself*. We can achieve this by using the method called `t2_img.get_fdata()`:

```python
t2_data = t2_img.get_fdata()
print(t2_data)
```

```output
array([[[0., 0., 0., ..., 0., 0., 0.],
        [0., 0., 0., ..., 0., 0., 0.],
        [0., 0., 0., ..., 0., 0., 0.],
        ...,
        [0., 0., 0., ..., 0., 0., 0.],
        [0., 0., 0., ..., 0., 0., 0.],
        [0., 0., 0., ..., 0., 0., 0.]],

       [[0., 0., 0., ..., 0., 0., 0.],
        [0., 0., 0., ..., 0., 0., 0.],
        [0., 0., 0., ..., 0., 0., 0.],
        ...,
        [0., 0., 0., ..., 0., 0., 0.],
        [0., 0., 0., ..., 0., 0., 0.],
        [0., 0., 0., ..., 0., 0., 0.]],

       ...,

       [[0., 0., 0., ..., 0., 0., 0.],
        [0., 0., 0., ..., 0., 0., 0.],
        [0., 0., 0., ..., 0., 0., 0.],
        ...,
        [0., 0., 0., ..., 0., 0., 0.],
        [0., 0., 0., ..., 0., 0., 0.],
        [0., 0., 0., ..., 0., 0., 0.]]])
```

The initial observation you might make is the prevalence of zeros in the image. This abundance of zeros might prompt concerns about the presence of any discernible content in the picture. However, when working with radiological images, it's important to keep in mind that these images frequently contain areas of air surrounding the objects of interest, which appear as black space.

What type of data is this exactly in a computational sense? We can determine this by calling the `type()` function on `t2_data`:

```python
print(type(t2_data))
```

```output
numpy.ndarray
```

The data is stored as a multidimensional **array**, which can also be accessed through the file's `dataobj` property:

```python
t2_img.dataobj
```

```output
<nibabel.arrayproxy.ArrayProxy at 0x20c63b5a4a0>
```
As you might guess there are differences in how your computer handles something made with dataobj and an actual array. These differences effect memory and processing speed. These are not trivial issues if you deal with a very large dataset of MRIs. You can save time and memory by being conscious about what is cached and using the dataobj property when dealing with slices of the array as detailed [here](https://nipy.org/nibabel/images_and_memory.html) 


:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge: Check Out Attributes of the Array

How can we see the number of dimensions in the `t2_data` array? Once again, all of the attributes of the array can be seen by typing `t2_data.` followed by <kbd>Tab</kbd>.

:::::::::::::::  solution

## Solution

```python
print(t2_data.ndim)
```

```output
3
```

`t2_data` contains 3 dimensions. You can think of the data as a 3D version of a picture (more accurately, a volume).

<img src="fig/numpy_arrays.png" alt="Numpy arrays" width="70%;"/>

:::::::::::::::::::::::::

Remember typical 2D pictures are made out of **pixels**, but a 3D MR image is made up of 3D cubes called **voxels**.

<img src="fig/mri_slices.jpg" alt="MRI slices" width="70%;"/>

What is the shape of the image?

:::::::::::::::  solution

## Solution

```python
print(t2_data.shape)
```

```output
(432, 432, 30)
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

The three numbers given here represent the number of values *along a respective dimension (x,y,z)*.
This image was scanned in 30 slices, each with a resolution of 432 x 432 voxels.
That means there are `30 * 432 * 432 = 5,598,720` voxels in total!

Let's see the type of data inside of the array.

```python
print(t2_data.dtype)
```

```output
dtype('float64')
```

This tells us that each element in the array (or voxel) is a floating-point number.  
The data type of an image controls the range of possible intensities.
As the number of possible values increases, the amount of space the image takes up in memory also increases.

```python
import numpy as np
print(np.min(t2_data))
print(np.max(t2_data))
```

```output
0.0
630641.0785522461
```

For our data, the range of intensity values goes from 0 (black) to more positive digits (whiter).


To examine the value of a specific voxel, you can access it using its indices. For example, if you have a 3D array `data`, you can retrieve the value of a voxel at coordinates (x, y, z) with the following syntax:

```python
value = data[x, y, z]
```
This will give you the value stored at the voxel located at the specified index `(x, y, z)`. Make sure that the indices are within the bounds of the array dimensions.

To inspect the value of a voxel at coordinates (9, 19, 2), you can use the following code:

```python
print(t2_data[9, 19, 2])
```

```output
0.
```

This command retrieves and prints the intensity value at the specified voxel. The output represents the signal intensity at that particular location.

Next, we will explore how to extract and visualize larger regions of interest, such as slices or arrays of voxels, for more comprehensive analysis.

## Working with Image Data

Slicing does exactly what it seems to imply. Given a 3D volume, slicing involves extracting a 2D **slice** from our data.

![](fig/T1w.gif){alt='T1 weighted'}

From left to right: sagittal, coronal and axial slices of a brain.

Let's select the 10th slice in the z-axis of our data:

```python
z_slice = t2_data[:, :, 9]
```

This is similar to the indexing we did before to select a single voxel. However, instead of providing a value for each axis, the `:` indicates that we want to grab *all* values from that particular axis.

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge: Slicing MRI Data

Select the 20th slice along the y-axis and store it in a variable.
Then, select the 4th slice along the x-axis and store it in another variable.

:::::::::::::::  solution

## Solution

```python
y_slice = t2_data[:, 19, :]
x_slice = t2_data[3, :, :]
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

We've been slicing and dicing images but we have no idea what they look like. In the next section we'll show you one way you can visualize it all together.

## Visualizing the Data

We previously inspected the signal intensity of the voxel at coordinates (10,20,3). Let's see what out data looks like when we slice it at this location. We've already indexed the data at each x-, y-, and z-axis. Let's use `matplotlib`:

```python
import matplotlib.pyplot as plt
%matplotlib inline

slices = [x_slice, y_slice, z_slice]

fig, axes = plt.subplots(1, len(slices))
for i, slice in enumerate(slices):
    axes[i].imshow(slice.T, cmap="gray", origin="lower")
```

Now, we're shifting our focus away from discussing our data to address the final crucial attribute of a NIfTI.

### 3. [Affine](https://nipy.org/nibabel/coordinate_systems.html)

The final important piece of metadata associated with an image file is the **affine matrix**, which indicates the position of the image array data in a reference space.

Below is the affine matrix for our data:

```python
t2_affine = t2_img.affine
print(t2_affine)
```

```output
array([[-9.25672975e-01,  2.16410652e-02, -1.74031337e-05,
         1.80819931e+02],
       [ 2.80924864e-06, -3.28338569e-08, -5.73605776e+00,
         2.11696911e+01],
       [-2.16410652e-02, -9.25672975e-01, -2.03403855e-07,
         3.84010071e+02],
       [ 0.00000000e+00,  0.00000000e+00,  0.00000000e+00,
         1.00000000e+00]])
```

To explain this concept, recall that we referred to coordinates in our data as (x,y,z) coordinates such that:

- x is the first dimension of `t2_data`
- y is the second dimension of `t2_data`
- z is the third dimension of `t2_data`

Although this tells us how to access our data in terms of voxels in a 3D volume, it doesn't tell us much about the actual dimensions in our data (centimetres, right or left, up or down, back or front).
The affine matrix allows us to translate between *voxel coordinates* in (x,y,z) and *world space coordinates* in (left/right, bottom/top, back/front).
An important thing to note is that in reality in which order you have:

- Left/right
- Bottom/top
- Back/front

Depends on how you've constructed the affine matrix; thankfully there is in depth coverage of the issue [the nibabel documentation](https://nipy.org/nibabel/coordinate_systems.html)
For most of the the data we're dealing with we use a RAS coordinate system so it always refers to:

- Right
- Anterior
- Superior


## Anatomy

When we describe imaging of any part of the body except the brain, as professionals we all agree on certain conventions. We rely on anatomical position as the basis of how we orient ourselves, and we expect the patient's right side to be on the left of our screen. 


To get a better view of MRI processing explore lessons from Carpentries Incubators; namely:

 1. [Introduction to Working with MRI Data in Python](https://carpentries-incubator.github.io/SDC-BIDS-IntroMRI/)
 2. [Introduction to dMRI](https://carpentries-incubator.github.io/SDC-BIDS-dMRI/)
 3. [Functional Neuroimaging Analysis in Python ](https://carpentries-incubator.github.io/SDC-BIDS-fMRI/)

