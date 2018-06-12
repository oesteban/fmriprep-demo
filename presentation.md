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

layout: true

## fMRIPrep walkthrough
### Reports

---

Reports describe the data as found, and the steps applied.

<p align="center">
<img alt="Execution and anatomical summary" src="assets/anat_summary.png">
</p>

Textual summaries are good to check for obvious failures, such as missing images or implausible
values.

---

<p>
<object class="svg-reportlet" type="image/svg+xml"
 data="assets/sub-001_T1w_seg_brainmask.svg">filename:assets/sub-001_T1w_seg_brainmask.svg</object>
</p>

The brain mask report shows the quality of intensity non-uniformity (INU) correction,
skull stripping, and tissue segmentation.

---

<p>
<object class="svg-reportlet" type="image/svg+xml"
 data="assets/sub-001_T1w_t1_2_mni.svg">filename:assets/sub-001_T1w_t1_2_mni.svg</object>
</p>

The MNI normalization report shows the quality of the non-linear normalization step.

Skull stripping defects may be more obvious here.

---

<p>
<object class="svg-reportlet" type="image/svg+xml"
 data="assets/sub-001_T1w_reconall.svg">filename:assets/sub-001_T1w_reconall.svg</object>
</p>

The FreeSurfer subject reconstruction report shows the white-gray boundary
and pial surface overlaid on the T1w image.

---
name: funcsum

<p align="center">
<img alt="Functional summary" src="assets/func_summary.png">
</p>

The functional summary can vary based on:

1. Available data
---
template: funcsum
 \- SDC technique depends on the available field maps

---
template: funcsum
2. Metadata - slice-timing correction requires `SliceTiming` metadata entry

---
template: funcsum
2. Metadata
3. User selections - `bbregister` requires FreeSurfer, `FLIRT` used otherwise

---
template: funcsum
2. Metadata
3. User selections
4. Heuristics - BBR may fall back to volume-based coregistration

---

<p>
<object class="svg-reportlet" type="image/svg+xml"
 data="assets/sub-001_task-stroop_bold_sdc_syn.svg">filename:assets/sub-001_task-stroop_bold_sdc_syn.svg</object>
</p>

The fieldmap-less susceptibility distortion correction (SDC) report shows
a before and after view, with the white matter segmentation overlaid as
reference.

---

<p>
<object class="svg-reportlet" type="image/svg+xml"
 data="assets/sub-001_task-stroop_bold_rois.svg">filename:assets/sub-001_task-stroop_bold_rois.svg</object>
</p>

BOLD ROI reports show the BOLD brainmask along with the aCompCor and
tCompCor masks.

---

<p>
<object class="svg-reportlet" type="image/svg+xml"
 data="assets/sub-001_task-stroop_bold_bbr.svg">filename:assets/sub-001_task-stroop_bold_bbr.svg</object>
</p>

The boundary-based registration report shows the registered BOLD reference
with the white and pial surfaces overlaid.

---

<p align="center">
<img src="assets/sub-001_task-stroop_bold_carpetplot.svg" width="60%" />
</p>

The BOLD summary report shows several characteristic statistics along
with a carpetplot, giving a view of the temporal characteristics of the
preprocessed BOLD series.

---

<div style="height: 3em"></div>

#### Summary

1. Show researchers their data
2. Describe the preprocessing performed
3. Show the results of preprocessing, facilitating early error detection

---

layout: true

.left-column[

# fMRIPrep
### What is it?
### How do I use it?
### Walk-through
### But what about...

]

---
.right-column[## fMRIPrep is stable, not finished!]
--
.right-column[
* fMRI preprocessing is a moving target, and best practices are going to change.
]
--
.right-column[
* You may already have improvements you want to make.
]
--
.right-column[
* fMRIPrep is a community-supported effort, and your contributions are encouraged!
]

---

.right-column[

## Outside contributions

Significant components of current fMRIPrep functionality were contributed by
interested users who had needs we didn't yet support.

Large additions include:

* ICA-AROMA support
* Multi-echo BOLD
* CIFTI2 outputs
* Lesion masking

In addition, we've had numerous bug fixes and documentation updates from users.
]
---
.right-column[
## Citation Needed

We believe that code and documentation are academic and scientific
contributions that deserve citation.

![Zenodo](assets/zenodo.png)

Zenodo is an online repository that archives and assigns DOIs to software.
We encourage all contributors to add themselves to our author list.

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

