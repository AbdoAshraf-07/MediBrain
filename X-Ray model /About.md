Chest X-Ray Multi-Label Classification & Localization
1. Problem Overview
Diagnosing thoracic diseases from chest X-rays is a complex task that requires high medical expertise. The primary challenges include:

Multi-label Nature: A single patient scan can often present multiple pathologies simultaneously (e.g., Infiltration and Effusion).

Data Imbalance: Standard datasets are heavily skewed towards "No Finding" cases, making it difficult for models to learn rare diseases.

Black-box Limitation: Most standard AI models provide a diagnosis without explaining "where" they see the disease, which limits clinical trust.

This project aims to build an end-to-end system that not only classifies 14 different diseases but also localizes the suspicious regions using advanced explainability techniques.

2. Dataset
The project utilizes the NIH Chest X-ray Dataset:

Size: 112,120 X-ray images from over 30,000 unique patients.

Labels: 14 distinct thoracic pathologies including Atelectasis, Cardiomegaly, Consolidation, Edema, Effusion, Emphysema, Fibrosis, Hernia, Infiltration, Mass, Nodule, Pleural Thickening, Pneumonia, and Pneumothorax.

Metadata: Includes patient age, gender, and view position (AP/PA).

3. Data Preprocessing
To ensure high performance and model robustness, the following steps were implemented:

Smart Resampling: A weighted sample of 40,000 images was curated to prioritize rare pathological cases and reduce the dominance of healthy scans.

Advanced Augmentation: Real-time transformations including horizontal flipping, 5-degree rotations, 15% zooming, and shifting were applied to prevent overfitting.

Standardization: Images were normalized using sample-wise centering and standard deviation scaling to account for varying exposures across different X-ray machines.

Multi-label Encoding: Pathology labels were transformed into binary vectors to support simultaneous multi-disease prediction.

4. Model Architecture & Design
The core engine of this project is based on the DenseNet121 architecture:

Backbone: DenseNet121 was chosen for its "Dense Connectivity" pattern, which improves gradient flow and allows for the reuse of features, making it ideal for detecting subtle medical patterns.

Custom Classification Head: A specialized top layer consisting of GlobalAveragePooling2D, followed by BatchNormalization, and Dropout (0.5) to ensure stable training.

Activation: A Sigmoid output layer is used to provide independent probabilities for each of the 14 labels.

Localization (Grad-CAM): Integrated Gradient-weighted Class Activation Mapping to track the gradients of a specific class into the final convolutional layer to produce a heatmap of the decision-making area.

5. Evaluation & Results
The model is evaluated based on its clinical utility and statistical accuracy:

AUC-ROC: The primary target is an AUC score of > 0.80 for most classes, ensuring strong discriminative power.

Binary Accuracy: Aiming for > 90% overall accuracy across all binary label predictions.

Loss Function: Binary Cross-Entropy is used to penalize incorrect predictions for each disease independently.

Localization Validation: The generated Bounding Boxes are visually inspected to ensure they align with the physiological location of the predicted disease (e.g., the box should be around the heart for Cardiomegaly).
