# EE513 Project 3 Proposal

**Name:** Emmett Williamson

## Track and what I will build

**Track B, industrial inspection.** I will build a detector that flags defective metal nuts
in photos, using good examples to build a classical template and two feature-based detectors.  I would also like to attempt to bridge this project with my STAT 671 class by using a kernel method for scoring instead of the PatchCore nearest-neighbor distance.

- **Classical:** compare pixels against one averaged good image.
- **Learned (PatchCore):** a frozen, pretrained CNN describes each small patch of the image.
  A patch is anomalous if it is far from every patch in a memory bank built from good
  images. No network training is needed.
- **Kernel method:** use the same CNN features, but score a patch by its average similarity
  to good patches. Random Fourier features approximate that similarity using a compact
  summary of the good examples. This is a different score from PatchCore, not an exact
  compression of it, and connects to my STAT 671 project.

## Data

- **MVTec AD, `metal_nut` category** (public). It has 220 good training images and 115
  test images (22 good, 93 defective), with defect masks. **Current download status:
  [confirm].** I will hold out 40 good training images for tuning and threshold selection.
- **Measurement:** about 20 photos of one real nut, with a checkerboard in the scene
  for scale. These are not captured yet; they take one short session.

## Baseline

Golden-template subtraction:

1. Average the good images into a template.
2. Align each test image to the template.
3. Subtract the template and threshold the difference.
4. Clean up the result with morphology.
5. Reject the part if the largest connected blob is bigger than a minimum area.

I will tune the threshold and the minimum area on the held-out good images.

## Metric and operating point

- **Detection:** for each method, select a threshold targeting a **5% false-positive
  rate** on held-out good images. Report test false-positive and false-negative rates with
  95% binomial confidence intervals, alongside ROC curves with the operating points marked.
  The small holdout limits the precision of the target rate.
- **Measurement:** the nut's across-flats width in mm, from sub-pixel edge fits. The
  uncertainty is the standard deviation across the repeated photos.

## Deployed artifact

The pretrained backbone and Fourier-feature kernel scorer exported to ONNX and run with
onnxruntime on my laptop CPU at 256×256. I will report parts per second as mean ± std over
repeated end-to-end runs, discarding the first run and stating the hardware.

## Scope limits

One category and one backbone, shared by PatchCore and the kernel method. I will compare
their detection and speed with the classical baseline, and show at least three failure
crops and discuss whether the methods could invent or obscure defects.

## Most likely to sink the project

Scope and preparation.  Because I want to attempt to combine projects, the singluar scope feels larger than what it should be, but I'm also attempting to hit two birds with one stone.  Knowing that all of the work being done for the project will apply to both classes gives me confidence that I'll be able to get it done.  
