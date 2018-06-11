class: center middle

# fMRIPrep - A Robust Preprocessing Pipeline for functional MRI
### Christopher J. Markiewicz
#### Center for Reproducible Neuroscience
#### Stanford University

---

layout: false
.left-column[
# fMRIPrep
### What is it?
]
.right-column[
## Robust, generic fMRI preprocessing

* Works with the data you give it, imposing minimal requirements
  * You can run it on a T1w scan and a BOLD series

## Standardized

* By adhering to modern data format standards, fMRIPrep can detect
  the available images and prepare outputs that can be used by all
  major software packages (FSL, SPM, etc.)

## Self-documenting

* fMRIPrep generates reports that allow you to detect issues in
  preprocessing

]

---

.left-column[
# fMRIPrep
### What is it?
### How do I use it?
]
.right-column[
## Brain Imaging Data Structure
* fMRIPrep will run on any BIDS-formatted dataset ([What is BIDS?](https://bids.neuroimaging.io))


### Docker
```Bash
$ pip install fmriprep-docker
$ fmriprep-docker /data_dir /outputs_dir participant
```

### Singularity
```Bash
$ singularity exec docker://poldracklab/fmriprep:latest \
  /data_dir /outputs_dir participant
```

### [OpenNeuro.org](https://openneuro.org/)

* OpenNeuro is a free, online platform for sharing and analyzing neuroimaging data
]

---

layout: true

## fMRIPrep walkthrough

---

### Your data

.pull-left[![Example BIDS Dataset](assets/bids.png)]
.pull-right[
* BIDS datasets are organized by subject, modality, and image type.

* fMRIPrep uses T1-weighted images for anatomical preprocessing.

* BOLD series and field maps (if available) are used for
functional preprocessing.
]

---

### The pipeline

<div align="center" style='margin-top: 1em'>
<img alt="Example BIDS Dataset" src="assets/fmriprep-workflow-lowtext.png" width="85%">
</div>

---

### Derivatives

fMRIPrep outputs adhere to the
[BIDS Derivatives](https://docs.google.com/document/d/1Wwc4A6Mow4ZPPszDIWfCUCRNstn7d_zzaWPcfcHmgI4)
draft specification.

.pull-left[

#### Anatomical

* T1-weighted template
  * INU-corrected
  * Multiple images merged
* Anatomical brain mask
* Tissue probability/class maps
  * Classes: GM, WM, CSF
* T1w ⇄ MNI transforms
* Reconstructed surfaces\*
  * `smoothwm`, `pial`, `midthickness`, `inflated`
* Parcellation and segmentation (`aparcaseg`)\*

.footnote[\* Requires FreeSurfer]

]

.pull-right[

#### Functional

In each output space (e.g., T1w, MNI, surface\*):

* Distortion corrected BOLD series
* BOLD mask
* Confounds (tabular file)
* Non-aggressively denoised BOLD series\*\*
* MELOIC mix (tabular file)\*\*
* ICA-AROMA noise components (tabular file)\*\*

.footnote[\*\* Requires ICA-AROMA]

]
---
.left-column[

# fMRIPrep
### What is it?
### How do I use it?
### Walk-through
### But what about...

]

---
.right-column[
]

---

layout: false
## Summary

---

class: middle center
# Questions?

### Please visit [Poster #2035](https://files.aievolution.com/hbm1801/abstracts/31779/2035_Markiewicz.pdf)

---
## References

