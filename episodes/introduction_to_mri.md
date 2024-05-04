---
title: "Working with MRI"
teaching: 60
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- What kinds of MRI are there?
- How are MRI data represented digitally?
- What file structures should I use around MRIs for neuroimaging?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Learn common kinds of MRI imaging used in research
- Understand the most common file formats for MRI
- Introduce MRI coordinate systems
- Load an MRI scan into Python and understand how the data is stored
- View and manipulate image data
- Understand what BIDS is
- Understand advantages of working with Nifti and BIDS
- Know a method to convert from DICOM to BIDS/nifti

::::::::::::::::::::::::::::::::::::::::::::::::


## Introduction

This is a lesson created heavily drawing from other existing
Carpentries lessons; namely:

 1. [Introduction to Working with MRI Data in Python](https://carpentries-incubator.github.io/SDC-BIDS-IntroMRI/)
 2. [Introduction to dMRI](https://carpentries-incubator.github.io/SDC-BIDS-dMRI/)
 3. [Functional Neuroimaging Analysis in Python ](https://carpentries-incubator.github.io/SDC-BIDS-fMRI/)

We will not cover all the mateiral in these lessons, rather give an over view of key points.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Inline instructor notes :
If Zenodo is unavailable steer students to 
https://github.com/esciencecenter-digital-skills/med-image-ext
then work in a notebook from there, so use the following:
'../../data/geometry_medical_images/NIFTI/OBJECT_phantom_T2W_TSE_Cor_14_1.nii'

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::



## Types of MR scans

### Anatomical

![](fig/t1t2flairbrain.jpg)

*Sourced from [https://case.edu/med/neurology/NR/MRI%20Basics.htm](https://case.edu/med/neurology/NR/MRI%20Basics.htm)*

- 3D images of anatomy 
- different tissue types produce different intensities 

### Functional

![](fig/bold.gif)

![](fig/fmri_timeseries.png)

*Sourced from Wagner and Lindquist, 2015*

- reveals blood oxygen level-dependant (BOLD) signal 
- four dimensional image (x, y, z and time)

### Diffusion 

![](fig/dwi.gif)

![](fig/dwi_tracts.png)

*Sourced from [http://brainsuite.org/processing/diffusion/tractography/](https://brainsuite.org/processing/diffusion/tractography/)*

- measures diffusion of water in order to model tissue microstructure
- four dimensional images (x, y, z + direction of diffusion)
- has parameters about the strength of the diffusion "gradient" and its direction in `.bval` and `.bvec` files

### Other types of MRI

Perfusion weighted imaging includes relatively novel sequences such as dynamic contrast-enhanced  MR perfusion, dynamic susceptibility contrast  MR perfusion, and arterial spin labelled perfusion. 

MRI can also be used for spectroscopy, but this is not covered as it isn't a true image.

## Common MRI file formats

| Format Name | File Extension | Origin/Group                                  | More info|
| ----------- | -------------- | --------------------------------------------- |-----------
| DICOM       | none or .dc    | ACR/NEMA Consortium                           |https://www.dicomstandard.org/  |
| Analyze     | .img/.hdr      | Analyze Software, Mayo Clinic                 |https://eeg.sourceforge.net/ANALYZE75.pdf|
| NIfTI       | .nii           | Neuroimaging Informatics Technology Initiative|https://brainder.org/2012/09/23/the-nifti-file-format/|
| MINC        | .mnc           | Montreal Neurological Institute               |https://www.mcgill.ca/bic/software/minc|
| NRRD        | .nrrd          |                                               |https://teem.sourceforge.net/nrrd/format.html|

From the MRI scanner, images are initially collected in the DICOM format and can be converted to these other formats to make working with the data easier.


We will in a later episode look more deeplu into DICOM data. The DICOM will have all kinds of data
such as the patient's name. In this episode we want to get to the image.
We'll load some example images originally from Zenodo where you can find [this data](https://doi.org/10.5281/zenodo.6466491) and it's open lisence. We will use a **wget** command to get thess from Zeonodo.

```python
TODO: write wget command 
```



NIfTI is one of the most ubiquitous file formats for storing neuroimaging data.
We can convert DICOM data to NIfTI using [dcm2niix](https://github.com/rordenlab/dcm2niix).

We can learn how to run `dcm2niix` by taking a look at its help menu.

```bash
dcm2niix -help
```
One of the advaantages of working with dcm2niix is that it can be used to create BIDS structured files. Basically it will give you a NiFTI and a json  with metadata ready to fit into the BIDS standard. [BIDS](https://bids.neuroimaging.io/) is a widely adopted standard of how data from neuroimaging research can be organized. Issues of how data and files are organized are actually critical in terms of working across research groups, or even from one researcher to another. Some pipelines assume your data is organized in BIDS structure, and these are sometimes called [BIDS Apps](https://bids-apps.neuroimaging.io/apps/).

Some of the more popular examples are:

   
    fmriprep
    freesurfer
    micapipe
    SPM
    MRtrix3_connectome

 We reccomend you use the [BIDS starter-kit website](https://bids-standard.github.io/bids-starter-kit/#) to learn the basics if you need to learn the basics of this standard. 

We'll now cover some details on working with NiFTIS.

## Reading NIfTI images

[NiBabel](https://nipy.org/nibabel/) is a Python package for reading and writing neuroimaging data.
To learn more about how NiBabel handles NIfTIs, check out the [NiBabel documentation on working with Niftis](https://nipy.org/nibabel/nifti_images.html) from which this episode is heavily based.

```python
import nibabel as nib
```

First, use the `load()` function to create a NiBabel image object from a NIfTI file.


.

```python
t2_img = nib.load("../../data/geometry_medical_images/NIFTI/OBJECT_phantom_T2W_TSE_Cor_14_1.nii"")
```

Loading in a NIfTI file with `NiBabel` gives us a special type of data object which encodes all the information in the file.Each bit of information is called an **attribute** in Python's terminology.
To see all of these attributes, type `t2_img.` followed by <kbd>Tab</kbd>.
There are three main attributes that we'll discuss today:

### 1\. [Header](https://nipy.org/nibabel/nibabel_images.html#the-image-header): contains metadata about the image, such as image dimensions, data type, etc.

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

`t2_hdr` is a Python **dictionary**.
Dictionaries are containers that hold pairs of objects - keys and values. Let's take a look at all of the keys.
Similar to `t2_img` in which attributes can be accessed by typing `t2_img.` followed by <kbd>Tab</kbd>, you can do the same with `t2_hdr`.
In particular, we'll be using a **method** belonging to `t2_hdr` that will allow you to view the keys associated with it.

```python
t2_hdr.keys()
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

Notice that **methods** require you to include () at the end of them whereas **attributes** do not.
The key difference between a method and an attribute is:

- Attributes are stored *values* kept within an object
- Methods are *processes* that we can run using the object. Usually a method takes attributes, performs an operation on them, then returns it for you to use.

When you type in `t2_img.` followed by <kbd>Tab</kbd>, you'll see that attributes are highlighted in orange and methods highlighted in blue.

The output above is a list of **keys** you can use from `t2_hdr` to access **values**.
We can access the value stored by a given key by typing:

```python
t2_hdr['<key_name>']
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Extract values from the NIfTI header

Extract the value of pixdim from `t2_hdr`

:::::::::::::::  solution

## Solution

```python
t2_hdr['pixdim']
```

```output
array([1.  , 2.75, 2.75, 2.75, 1.  , 1.  , 1.  , 1.  ], dtype=float32)
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### 2\. Data

As you've seen above, the header contains useful information that gives us information about the properties (metadata) associated with the MR data we've loaded in.
Now we'll move in to loading the actual *image data itself*.
We can achieve this by using the method called `t2_img.get_fdata()`.

```python
t2_data = t2_img.get_fdata()
t2_data
```

```output
array([[[ 8.79311806,  8.79311806,  8.79311806, ...,  7.73574093,
          7.73574093,  7.38328189],
        [ 8.79311806,  9.1455771 ,  8.79311806, ...,  8.08819997,
          8.08819997,  8.08819997],
        [ 9.1455771 ,  8.79311806,  8.79311806, ...,  8.44065902,
          8.44065902,  8.44065902],
        ...,
        [ 8.08819997,  8.44065902,  8.08819997, ...,  7.38328189,
          7.38328189,  7.38328189],
        [ 8.08819997,  8.08819997,  8.08819997, ...,  7.73574093,
          7.38328189,  7.38328189],
        [ 8.08819997,  8.08819997,  8.08819997, ...,  7.38328189,
          7.38328189,  7.03082284]],

       [[ 8.79311806,  9.1455771 ,  8.79311806, ...,  7.73574093,
          7.38328189,  7.38328189],
        [ 8.79311806,  9.1455771 ,  9.1455771 , ...,  8.08819997,
          7.73574093,  8.08819997],
        [ 8.79311806,  9.49803615,  9.1455771 , ...,  8.44065902,
          8.44065902,  8.44065902],
        ...,
        [ 8.08819997,  8.08819997,  8.08819997, ...,  7.38328189,
          7.38328189,  7.03082284],
        [ 8.08819997,  8.08819997,  8.08819997, ...,  7.38328189,
          7.38328189,  7.38328189],
        [ 8.08819997,  8.08819997,  8.08819997, ...,  7.38328189,
          7.38328189,  7.73574093]],

       [[ 9.1455771 ,  9.1455771 ,  8.79311806, ...,  7.73574093,
          7.38328189,  7.03082284],
        [ 9.1455771 ,  9.49803615,  9.1455771 , ...,  8.08819997,
          7.73574093,  7.38328189],
        [ 9.1455771 ,  9.49803615,  9.1455771 , ...,  8.08819997,
          8.08819997,  8.08819997],
        ...,
        [ 8.08819997,  8.44065902,  8.44065902, ...,  7.73574093,
          7.38328189,  7.38328189],
        [ 8.44065902,  8.08819997,  8.44065902, ...,  7.38328189,
          7.38328189,  7.38328189],
        [ 8.08819997,  8.08819997,  8.08819997, ...,  7.38328189,
          7.38328189,  7.38328189]],

       ...,

       [[ 9.49803615,  9.85049519,  9.85049519, ...,  7.38328189,
          7.38328189,  7.03082284],
        [ 9.85049519,  9.85049519,  9.85049519, ...,  7.38328189,
          7.38328189,  7.73574093],
        [ 9.85049519,  9.85049519, 10.20295423, ...,  8.08819997,
          8.08819997,  8.08819997],
        ...,
        [ 9.49803615,  9.1455771 ,  9.49803615, ...,  7.73574093,
          7.73574093,  7.73574093],
        [ 9.49803615,  9.49803615,  9.1455771 , ...,  7.73574093,
          8.08819997,  7.73574093],
        [ 9.49803615,  9.1455771 ,  8.79311806, ...,  8.08819997,
          7.73574093,  7.73574093]],

       [[ 9.49803615,  9.85049519, 10.20295423, ...,  7.38328189,
          7.38328189,  7.03082284],
        [ 9.1455771 ,  9.85049519,  9.1455771 , ...,  7.73574093,
          7.38328189,  7.38328189],
        [ 9.85049519,  9.85049519,  9.49803615, ...,  8.08819997,
          7.73574093,  8.08819997],
        ...,
        [ 9.49803615,  8.79311806,  9.1455771 , ...,  8.08819997,
          7.38328189,  7.73574093],
        [ 9.1455771 ,  9.1455771 ,  9.1455771 , ...,  7.73574093,
          8.08819997,  7.73574093],
        [ 8.79311806,  9.1455771 ,  9.1455771 , ...,  7.73574093,
          7.73574093,  7.38328189]],

       [[ 9.1455771 ,  8.79311806,  8.79311806, ...,  7.03082284,
          7.03082284,  6.6783638 ],
        [ 9.49803615,  9.85049519,  9.49803615, ...,  7.73574093,
          7.73574093,  7.73574093],
        [ 9.49803615,  9.49803615,  9.85049519, ...,  7.73574093,
          8.08819997,  7.73574093],
        ...,
        [ 8.79311806,  9.1455771 ,  9.1455771 , ...,  8.08819997,
          8.08819997,  8.08819997],
        [ 9.1455771 ,  9.1455771 ,  9.1455771 , ...,  8.08819997,
          8.08819997,  7.73574093],
        [ 8.79311806,  9.1455771 ,  9.1455771 , ...,  8.08819997,
          7.73574093,  7.73574093]]])
```

What type of data is this exactly? We can determine this by calling the `type()` function on `t2_data`.

```python
type(t2_data)
```

```output
numpy.ndarray
```

The data is a multidimensional **array**.

:::::::::::::::::::::::::::::::::::::::  challenge

## Check out attributes of the array

How can we see the number of dimensions in the `t2_data` array? Once again, all of the attributes of the array can be seen by typing `t2_data.` followed by <kbd>Tab</kbd>.

:::::::::::::::  solution

## Solution

```python
t2_data.ndim
```

```output
3
```

`t2_data` contains 3 dimensions. You can think of the data as a 3D version of a picture (more accurately, a volume).
![](fig/numpy_arrays.png)


:::::::::::::::::::::::::

Remember typical 2D pictures are made out of **pixels**, but a 3D MR image is made up of 3D cubes called **voxels**.
![](fig/mri_slices.jpg)  
How big each dimension is (shape)?

:::::::::::::::  solution

## Solution

```python
t2_data.shape
```

```output
(57, 67, 56)
(432, 432, 30)
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

The 3 numbers given here represent the number of values *along a respective dimension (x,y,z)*.
This image was scanned in 30 slices with a resolution of 432 x 432 voxels per slice.
That means there are:

`30 * 432 * 432 = 5,598,720`

voxels in total!

Let's see the type of data inside of the array.

```python
t2_data.dtype
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
6.678363800048828
96.55541983246803
```

For our data, the range of intensity values goes from 0 (black) to more positive digits (whiter).

How do we examine what value a particular voxel is?
We can inspect the value of a voxel by selecting an index as follows:

`data[x,y,z]`

So for example we can inspect a voxel at coordinates (10,20,3) by doing the following:

```python
t2_data[9, 19, 2]
```

```output
32.40787395834923
```

This yields a single value representing the intensity of the signal at a particular voxel!
Next we'll see how to not just pull one voxel but a slice or an array of voxels for visualization and analysis!

## Working with image data

Slicing does exactly what it seems to imply.
Giving our 3D volume, we pull out a 2D **slice** of our data.

![](fig/T1w.gif)
From left to right: sagittal, coronal and axial slices.

Let's pull the 10th slice in the x axis.

```python
x_slice = t2_data[9, :, :]
```

This is similar to the indexing we did before to pull out a single voxel.
However, instead of providing a value for each axis, the `:` indicates that we want to grab *all* values from that particular axis.

:::::::::::::::::::::::::::::::::::::::  challenge

## Slicing MRI data

Now try selecting the 20th slice from the y axis.

:::::::::::::::  solution

## Solution

```python
y_slice = t2_data[:, 19, :]
```

:::::::::::::::::::::::::

Finally, try grabbing the 3rd slice from the z axis

:::::::::::::::  solution

## Solution

```python
z_slice = t2_data[:, :, 2]
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

We've been slicing and dicing brain images but we have no idea what they look like! In the next section we'll show you how you can visualize brain slices!

## Visualizing the data

We previously inspected the signal intensity of the voxel at coordinates (10,20,3).
Let's see what out data looks like when we slice it at this location.
We've already indexed the data at each x, y, and z axis.
Let's use `matplotlib`.

```python
import matplotlib.pyplot as plt
%matplotlib inline

slices = [x_slice, y_slice, z_slice]

fig, axes = plt.subplots(1, len(slices))
for i, slice in enumerate(slices):
    axes[i].imshow(slice.T, cmap="gray", origin="lower")
```

Now, we're going to step away from discussing our data and talk about the final important attribute of a NIfTI.

### 3\. [Affine](https://nipy.org/nibabel/coordinate_systems.html): tells the position of the image array data in a reference space

The final important piece of metadata associated with an image file is the **affine matrix**.
Below is the affine matrix for our data.

```python
t2_affine = t2_img.affine
t2_affine
```

```output
array([[  2.75,   0.  ,   0.  , -78.  ],
       [  0.  ,   2.75,   0.  , -91.  ],
       [  0.  ,   0.  ,   2.75, -91.  ],
       [  0.  ,   0.  ,   0.  ,   1.  ]])
```

To explain this concept, recall that we referred to coordinates in our data as (x,y,z) coordinates such that:

- x is the first dimension of `t2_data`
- y is the second dimension of `t2_data`
- z is the third dimension of `t2_data`

Although this tells us how to access our data in terms of voxels in a 3D volume, it doesn't tell us much about the actual dimensions in our data (centimetres, right or left, up or down, back or front).
The affine matrix allows us to translate between *voxel coordinates* in (x,y,z) and *world space coordinates* in (left/right,bottom/top,back/front).
An important thing to note is that in reality in which order you have:

- left/right
- bottom/top
- back/front

Depends on how you've constructed the affine matrix, but for the data we're dealing with it always refers to:

- Right
- Anterior
- Superior

Applying the affine matrix (`t2_affine`) is done through using a *linear map* (matrix multiplication) on voxel coordinates (defined in `t2_data`).

![](fig/coordinate_systems.png)

The concept of an affine matrix may seem confusing at first but an example might help gain an intuition:

Suppose we have two voxels located at the the following coordinates:
$(64, 100, 2)$

And we wanted to know what the distances between these two voxels are in terms of real world distances (millimetres).
This information cannot be derived from using voxel coordinates so we turn to the **affine matrix**.

Now, the affine matrix we'll be using happens to be encoded in **RAS**.
That means once we apply the matrix our coordinates are as follows:  
`(Right, Anterior, Superior)`

So increasing a coordinate value in the first dimension corresponds to moving to the right of the person being scanned.

Applying our affine matrix yields the following coordinates:
$(10.25, 30.5, 9.2)$

This means that:

- Voxel 1 is `90.23 - 10.25 = 79.98` in the R axis. Positive values mean move right
- Voxel 1 is `0.2 - 30.5 = -30.3` in the A axis. Negative values mean move posterior
- Voxel 1 is `2.15 - 9.2 = -7.05` in the S axis. Negative values mean move inferior

## Functional MRI data

Functional MRI data is inherently noisy, as people move thier heads. Usually we are interested in grey matter brain cells, but other cells and structures can also generate signal. Filtering and many other techniques are used to clean up fMRI data. Although this sort of imaging is quite difficult to interpret, the effort itself has brought the neuroimaging community many positive outcomes. For example [fMRIPrep](https://github.com/nipreps/fmriprep) was a model across new modalities,and now we have the general concept of [nipreps]( https://www.nipreps.org/). And by the way, fmriprep is still the go-to package for this difficult work. If you are less interested in coding, but still need it to accomplish your research goals, it can be worthwhile to use packages that are well known, as it is easier to find various forms of documentation and help. For this reason [nilearn](https://github.com/nilearn/nilearn) is a library to consider for fMRI data.

:::::::::::::::: callout

### Adantages of nilearn:

-Fully free and open source

-Extremely popular

-Allows Python coding

-Implementations of many state-of-the art algorithms

-Works on Nibabel objects

::::::::::::::
## Diffusion MRI data

Diffusion MRIs have additional data when compared to anatomical MRIs.

Diffusion sequences which are sensitive to the signals from the random, microscropic motion (i.e. diffusion) of water protons. The diffusion of water in anatomical structures is restricted due to barriers (e.g. cell membranes), resulting in a preferred direction of diffusion (anisotropy). A typical diffusion MRI scan will acquire multiple volumes with varying magnetic fields which are sensitive to diffusion along a particular direction and result in diffusion-weighted images.
In addition to the acquired images, two files are collected as part of the diffusion dataset, known as the b-vectors and b-values. The b-value (file suffix .bval) is the diffusion-sensitizing factor, and reflects the diffusion gradient timing and strength. The b-vector (file suffix .bvec) corresponds to the direction with which diffusion was measured. Together, these two files define the diffusion MRI measurement as a set of gradient directions and corresponding amplitudes, and are necessary to calculate useful measures of the microscopic properties. 

Depending open what you want to do with your imaging you may use a pre-contructed pipeline only, or you may want to code.
A strong possible library for coding with diffusion images is [the DIPY (Diffusion Imaging in Python) package](https://dipy.org/index.html#)

::::::::::::::: callout

## Adantages of DIPY:

-Fully free and open source

-Allows Python coding

-Implementations of many state-of-the art algorithms

-Has methods for diffusion tensor imaging

-High performance with many algorithms actually implemented in Cython under the hood

:::::::::::::::::::::

Tractography is a reconstruction technique to assess neural fiber tracts using the data of diffusion tensor imaging.
Tractography models axonal trajectories as 'streamlines' from local directional information. There are several families methods for tractopraphy. No known methods is exact and perfect, they all have biases and limitations. The streamlines generated by a tractography method and the required meta-data are usually saved into files called tractograms. 

:::::::::::::::::::::::::::::::::::::::: keypoints

- imaging MRIs commonly used for research can be anatomical, functional or diffusion
- MRIs can be converted from DICOMs to Niftis
- BIDS is a standard about organizing neuroimaging data
- NIfTI images contain a header, which describes the contents, and the data.
- The position of the NIfTI data in space is determined by the affine matrix.
- NIfTI data is a multi-dimensional array of values.
- diffusion MRI has b-values and b-vectors
- there are many various tractography methods, each with imperfections
- functional MRI requires heavy processing

::::::::::::::::::::::::::::::::::::::::::::::::::