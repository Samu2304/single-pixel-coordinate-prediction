## Problem Statement

Given a 50×50 grayscale image in which exactly one pixel has a value of 255 and all other pixels are 0, the task is to predict the (x, y) coordinates of that pixel using Deep Learning techniques.

The pixel with value 255 is randomly assigned for each image.

A dataset may be generated as required, and the rationale behind dataset design and modeling choices must be explained.

## My Understanding of the Problem

From the problem statement, I understood that:

The input is a small grayscale image (50×50)

There is only one active (bright) pixel

The position of this pixel is random

The output is not a class, but a pair of numerical coordinates (x, y)

This makes the problem a supervised regression task, where the goal is to learn a mapping from image space -> spatial coordinates.

The core challenge is not accuracy, but correct problem formulation and reasoning, as emphasized in the evaluation criteria.

![WhatsApp Image 2025-12-27 at 18 22 57](https://github.com/user-attachments/assets/edb502e6-9d8a-44cc-8ad2-39acff4edf8c)


## Dataset Design and Rationale
### Why Synthetic Data?

There is no real-world dataset for this exact problem, and one is not required.

The task is fully deterministic and well-defined, making synthetic data the most logical choice.

### Benefits of synthetic data:

Full control over labels

No noise or ambiguity

Guaranteed correctness

Easy scalability

### How the Dataset Is Generated

For each sample:

A 50×50 black image is created

One pixel location (x, y) is chosen randomly

That pixel is set to value 255 (normalized to 1.0)

The label is the normalized coordinate (x/50, y/50)

This process directly mirrors the problem statement.

### Why Normalization?

Pixel values are normalized from 255 -> 1.0

Coordinates are normalized to the range [0, 1]

Normalization improves:

Numerical stability

Training convergence

Compatibility with sigmoid output activation

## Approach Selection
Possible Approaches Considered
1. Rule-Based Pixel Scanning

Simply find the non-zero pixel using logic

Rejected because does not use Deep Learning

2. Fully Connected Neural Network

Flatten the image and regress coordinates

Rejected because ignores spatial structure of images

3. Classification-Based CNN (2500 Classes)

Treat each pixel position as a class

Rejected because unnecessarily complex and inefficient

### Chosen Approach: CNN-Based Regression

A Convolutional Neural Network (CNN) is used to:

Learn spatial features from the image

Regress directly to (x, y) coordinates

This approach is:

Simple and efficient

Scalable

Aligned with real-world localization tasks

Easy to interpret and visualize

<img width="102" height="484" alt="d1uu" src="https://github.com/user-attachments/assets/858a8c01-e369-411f-97bb-c881771caa36" />


## Model Architecture 

The model consists of:

Convolution layers for spatial feature extraction

Pooling layers for dimensionality reduction

Fully connected layers for coordinate regression

Sigmoid activation to constrain outputs to [0, 1]

<img width="102" height="1001" alt="Untitled (9)" src="https://github.com/user-attachments/assets/96523d1b-2ae6-4c8f-b1e5-c7b626f1a5b6" />


### Final output:

(x, y)->normalized pixel coordinates

## Training Strategy

Loss Function: Mean Squared Error (MSE)

Optimizer: Adam

Task Type: Supervised regression

Hardware: CPU-only (GPU not required)

Training and validation losses are tracked across epochs to ensure:

Stable learning

No overfitting

Proper generalization

## Evaluation and Visualization

### Model performance is evaluated using:

Loss Curves

Training vs validation loss across epochs

Prediction Visualization

Ground truth pixel location (green circle)
'
Predicted pixel location (red cross)

<img width="952" height="457" alt="image" src="https://github.com/user-attachments/assets/e80cb02a-0140-40a1-bc3f-2f9530c8135d" />

<img width="1381" height="350" alt="image" src="https://github.com/user-attachments/assets/3ce41d24-9a3f-4a5c-ba1a-0773ba21e1f9" />


### Visual inspection confirms that:

Predictions closely match ground truth

Errors are minimal (often within 1 pixel)

The model behaves as expected

### Does the Model Do the Job?
Yes.

The model correctly predicts pixel coordinates

It generalizes to unseen data

It satisfies all functional requirements

As stated in the assignment, approach and reasoning are prioritized over raw accuracy, and this solution fulfills that objective.
