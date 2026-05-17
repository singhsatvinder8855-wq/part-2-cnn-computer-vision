## part-2-cnn-computer-vision

# Part 2: Computer Vision Problem Formulation and CNN Prototype

## Task 1: Problem Identification

### Selected Problem Type:
**Image Classification**

### Why this is appropriate for the dataset:
The given dataset represents an **Image Classification** problem because the primary target is to analyze an entire input image and assign it a single discrete class label (`normal`, `scratch`, `dent`, or `stain`). 
- **Object Detection** is not appropriate here because we do not need to locate defects using specific bounding box coordinate points.
- **Semantic/Instance Segmentation** is not required because we do not need to map or trace boundaries pixel-by-pixel. 
Since the objective is to determine the overall condition state of the product surface from the image as a whole, Image Classification is the correct and most efficient approach.

---

## Task 2 & 3: Dataset Exploration & Preprocessing Summary
- **Total Images:** 456 actual valid images were dynamically found and verified inside the folder framework (`scratch: 116`, `normal: 118`, `dent: 116`, `stain: 106`).
- **Resizing:** All incoming images are programmatically resized to a uniform dimension of `128x128` pixels before entering the neural network layers.
- **Normalization:** Pixel integer intensities are rescaled from `[0, 255]` to a floating decimal range of `[0.0, 1.0]` by multiplying by `1./255` to ensure stable gradient tracking.
- **Data Augmentation:** To maximize structural robustness and reduce overfitting risks, randomized rotation variations, horizontal flips, and width/height translations are applied.
- **Data Splitting:** The images are divided using a stratified split: 80% is allocated for the training/validation pipeline and 20% is completely isolated as the test set.

---

## Task 4 & 5: CNN Architecture & Training Results
The built prototype cascades 3 distinct Convolutional block sets (`Conv2D` + `MaxPooling2D`). These feed directly into a `Flatten` conversion layer, followed by a fully-connected hidden layer (`Dense`) utilizing `Dropout` regularization to suppress memorization, and a final 4-node output layer configured with a `Softmax` activation function.
*(Once execution finishes, all visual loss tracking curves, confusion matrices, and inference image charts are automatically saved in your `/results` and `/sample_predictions` directories).*

---

## Task 6: CNN Concept Explanation

### 1. What is convolution?

Convolution is a mathematical operation where a small matrix of weights (known as a filter or kernel) slides systematically across the raw input image pixels. At each step, it performs element-wise multiplications and sums up the values to create a new condensed matrix called a "Feature Map". This allows the network to automatically extract vital visual features such as edges, textures, and lines.

### 2. Why is pooling used?

Pooling layers (most commonly Max Pooling) downsample and compress the spatial dimensions (height and width) of the extracted feature maps. This serves two main critical purposes:
- It drastically reduces the number of parameters and computational overhead in the model, speeding up training times.
- It introduces "Translation Invariance", meaning the network can still accurately recognize a feature or defect even if its exact position shifts slightly within the frame.

### 3. Why is ReLU commonly used in CNNs?
The Rectified Linear Unit (ReLU) is an activation function mathematically defined as $f(x) = \max(0, x)$, which converts all negative pixel values to zero. It is widely preferred in deep learning because:
- It effectively introduces non-linearity into the network, enabling the model to learn highly complex visual patterns.
- It computes significantly faster than older functions (like sigmoid or tanh) and completely avoids the "Vanishing Gradient Problem" during backpropagation passes.

### 4. Why are CNNs better than regular feed-forward networks for image data?
Regular feed-forward networks require flattening a multi-dimensional image into a giant 1D vector right at the beginning. This process completely destroys the native 2D spatial relationships (the local context of neighboring pixels is lost). Furthermore, full dense connections cause parameter counts to explode uncontrollably on high-resolution data. CNNs maintain spatial awareness through local connectivity and use weight-sharing to keep calculations light and efficient.

---

## Task 7: Business Use Case Mapping

### Domain: Manufacturing (Automated Quality Inspection)
This computer vision solution maps directly to automated defect detection on modern factory production lines. 

High-definition cameras mounted right above assembly conveyors can stream continuous live images of manufactured product surfaces straight into this custom CNN classification model. If the system flags an item containing a surface abnormality (such as a `scratch`, `dent`, or `stain`), automated line switches can immediately pull the defective unit out of the primary batch. This entirely eliminates slow and error-prone manual human checking, cuts down quality overhead costs, and ensures that only flawless products reach the final consumers.
