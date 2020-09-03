# Original vs Derived objects

We differentiate between the _original_ and _derived_ DICOM objects in the IDC portal and discussions of the IDC-hosted data. By Original objects we mean DICOM objects that are produced by image acquisition equipment - MR, CT or PET images fall into this category. By Derived objects we mean those objects that were generated by means of analysis or annotation of the original objects. Those objects can contains, for example, volumetric segmentations of the structures in the original images, or quantitative measurements of the objects in the image.

{% hint style="warning" %}
The search portal of IDC aimed to group attributes into categories that are specific to the original and derived objects. Due to the time constraints, the current listing of attributes as of IDC MVP does not correspond to the final desired organization, and is expected to change.
{% endhint %}

### Original objects

Most of the images stored on IDC are saved as objects that store individual slices of the image in separate instances of a series, with the image stored in the `PixelData` attribute. 

Open source libraries such as DCMTK, GDCM, ITK, and pydicom can be used to parse such files and load pixel data of the individual slices. Recovering geometry of the individual slices \(spatial location and resolution\) and reconstruction of the individual slices into a volume requires some extra consideration.

{% hint style="danger" %}
It is not safe to sort individual instances using file name, or any attribute other than those that communicate image geometry \(`ImagePositionPatient`, `ImageOrientationPatient`, and `PixelSpacing`\). This may result in accurate geometric ordering of slices!
{% endhint %}

We point this out, because even some prominent examples utilize oversimplified approaches to of recovering image geometry from DICOM files. As an example, this [DICOM data preprocessing tutorial from Kaggle](https://www.kaggle.com/gzuidhof/full-preprocessing-tutorial) uses `ImagePositionPatient` alone to infer slice spacing. This approach will result in incorrect computed slice spacing for oblique acquisitions \(i.e., see example below\).

![Fallacy of using ImagePositionPatient\[2\] for calculating slice spacing](../.gitbook/assets/spacing_issue.png)

Although in most cases a DICOM image series will correspond to a single traversal of a 3-d volume, in the general case case it may have multiple slices for the same spatial location \(e.g., for temporally-resolved acquisitions\). It is also, unfortunately, possible that DICOM series, as available to you, will have missing slices and, as a result, inconsistent spacing between slices.

You can use one of the existing tools to reconstruct image volume instead of implementing sorting of the slices on your own:

* dcm2niix
* plastimatch
* ITK readers?

If you want to sort slices on your own, you can see examples of how this can be done in the following places:

* Slicer - Python
* DCMTK - somewhere in util

### Derived objects

* add references to dcmqi, highdicom
* reference DICOM4QI for what are the other tools that support




