
## CHROMA-KEYING(REMOVING BACKGROUND(GREEN) FROM THE ORIGINAL FOREGROUND)
# Chroma Keying and Green Screen Segmentation

## Overview

This project explores the problem of chroma keying, the process of separating foreground objects from a green-screen background. While background removal in static images is relatively straightforward, designing a method that remains effective across multiple images or video frames is significantly more challenging due to lighting variations, shadows, foreground spill, and differences in green-screen quality.

The project investigates multiple segmentation strategies, analyzes their strengths and limitations, and proposes a ratio-based approach for robust foreground-background separation.

---

## Problem Statement

Traditional chroma keying methods often rely on manually selected RGB thresholds. These thresholds are highly sensitive to:

- Lighting conditions
- Shadows and reflections
- Variations in green-screen color
- Foreground interaction with the background
- Frame-to-frame changes in videos

The objective of this project is to develop a computationally efficient chroma-keying algorithm that minimizes foreground loss while ensuring complete removal of the green-screen background.

---

## Objectives

- Develop algorithms for foreground-background separation.
- Compare multiple chroma-keying approaches.
- Analyze edge cases and failure scenarios.
- Identify intrinsic properties of green screens.
- Propose a metric for evaluating green-screen quality.
- Improve segmentation robustness under varying lighting conditions.

---

## Methodology

### Approach 1: Histogram-Based Segmentation

The first approach uses RGB histograms to identify dominant background colors.

#### Idea

Since the green screen occupies a large portion of the image, its color distribution creates dominant peaks in the RGB histograms.

#### Workflow

1. Compute RGB histograms.
2. Detect dominant peaks.
3. Estimate threshold values.
4. Remove pixels belonging to the dominant color region.

#### Limitations

- Difficult to determine optimal thresholds.
- Thresholds vary significantly between images.
- Uniform foreground regions may be misclassified.
- Not suitable for video processing.

---

### Approach 2: RGB Threshold-Based Segmentation

The second approach incorporates domain knowledge of green-screen backgrounds.

#### Observation

For background pixels:

- Green intensity is relatively high.
- Red intensity is relatively low.
- Blue intensity is relatively low.

#### Segmentation Rule

```python
if green > threshold_g and red < threshold_r and blue < threshold_b:
    pixel = background
```

This approach improves segmentation quality compared to histogram-based methods.

#### Limitations

- Threshold selection remains empirical.
- Performance varies across images.
- Difficult to quantify segmentation quality.
- Sensitive to illumination changes.

---

### Approach 3: Ratio-Based Green Detection

To improve robustness, RGB ratios were used instead of absolute RGB values.

#### Core Idea

For green-screen pixels:

```text
g/r > Super Bound
g/b > Super Bound
```

where:

- g = Green channel intensity
- r = Red channel intensity
- b = Blue channel intensity

A pixel is classified as background only if both ratios exceed a predefined threshold.

---

## The Super Bound

The Super Bound is a threshold that determines whether a pixel belongs to the green screen.

Experimental analysis showed that:

```text
Super Bound ≈ 1.2
```

works effectively for most images.

Advantages:

- More robust to lighting variations.
- Less dependent on absolute color values.
- Generalizes better across different images and video frames.

---

## Green Screen Rating

A key contribution of this project is the introduction of a Green Screen Rating metric.

The rating is based on:

- Minimum g/r ratio
- Minimum g/b ratio
- Variation in green intensity across the background

A higher rating indicates:

- Better background consistency
- Improved segmentation performance
- Lower foreground loss

This provides a systematic way to evaluate green-screen quality.

---

## Experimental Analysis

Multiple green-screen images were tested under varying conditions.

### Key Findings

- Histogram-based segmentation is unstable.
- RGB thresholding improves accuracy but requires manual tuning.
- Ratio-based segmentation consistently performs better.
- The Super Bound remains relatively stable across datasets.
- Green-screen quality can be characterized using color-ratio statistics.

---

## Challenges

### Foreground Spill

Green reflections on foreground objects may be incorrectly removed.

### Shadows

Lighting inconsistencies affect pixel intensities and segmentation quality.

### Fine Structures

Hair strands and thin objects are difficult to separate accurately.

### Transparent Objects

Glass and translucent materials remain challenging for threshold-based methods.

---

## Technology Stack

- Python
- OpenCV
- NumPy
- Matplotlib
- Image Processing
- Color Space Analysis

---

## Results

The ratio-based chroma-keying approach significantly improved segmentation quality compared to conventional threshold-based methods.

Key outcomes include:

- Improved robustness against lighting variations.
- Reduced dependence on manually selected RGB thresholds.
- Introduction of a quantitative Green Screen Rating metric.
- More reliable foreground-background separation across multiple datasets.

---

## Future Work

- Automatic Super Bound estimation.
- Adaptive thresholding for video streams.
- Green-spill correction techniques.
- Hair-aware and transparency-aware segmentation.
- Extension to blue-screen and multi-color backgrounds.
- Machine learning-based foreground refinement.

---

## Learning Outcomes

This project provided practical experience in:

- Digital Image Processing
- Computer Vision
- Chroma Keying
- Color Space Analysis
- Threshold Optimization
- Error Analysis
- Algorithm Design
- Experimental Evaluation

---

## Conclusion

This project demonstrates that ratio-based color analysis provides a more robust framework for chroma keying than traditional RGB thresholding methods. By introducing the concepts of Super Bound and Green Screen Rating, the project establishes a systematic methodology for evaluating and improving green-screen segmentation under realistic conditions.

The work highlights how careful analysis of color distributions and intrinsic screen properties can lead to efficient and interpretable computer vision solutions without relying on complex machine learning models.>
