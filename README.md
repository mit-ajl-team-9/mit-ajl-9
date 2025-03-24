# mit-ajl-9

## Project Overview
### Kaggle Competition and Break Through Tech AI Program
This project is part of the Spring 2025 Break Through Tech AI Studio, a collaboration between Break Through Tech and the Algorithmic Justice League (AJL). It involves the Kaggle competition "Equitable AI for Dermatology," which focuses on building inclusive dermatology AI models by training on a diverse dataset of skin conditions and skin tones. The competition aims to give the Virtual, Boston (MIT), and Los Angeles (UCLA) cohort hands-on experience with machine learning while addressing real-world social justice issues. 

### Objective of the Challenge
The first goal is to develop a multi-class classification model that can accurately identify 21 skin conditions across a diverse range of skin tones using a subset of the FitzPatrick17k dataset provided in Kaggle. The competition uses a weighted average F1 score for evaluation to address class imbalances. We want to build a model with a high F1 score as well as high accuracy. In addition to focusing on overall model accuracy, another goal is to consider how dermatology AI models impact populations who have historically been marginalized and excluded in healthcare, possibly by using fairness and explainability tools, visualizations, and/or creative storytelling techniques to show how we have centered the “excoded” in the work.

### Real-World Significance and Potential Impact
Current dermatology AI systems are often trained on non-diverse datasets, leading to underperformance for people with darker skin tones. This perpetuates health disparities by causing misdiagnoses, delayed treatments, and poorer health outcomes for marginalized groups. By developing a model that performs equitably across all skin tones, this project has the potential to 1) reduce diagnostic errors for underrepresented skin types, 2) improve healthcare outcomes by ensuring timely and accurate diagnoses, and 3) promote fairness and accountability in AI applications, aligning with AJL's mission to prevent algorithmic harm.

## User Guide
1. Download the ipynb file (Newest version on Mar. 22, 2025: pending-submission-4)
2. Upload it to Google Colab
3. Download the data zip from Kaggle and upload it to Google Drive (under MyDrive)
4. Go to this link and download this code locally, then upload same code to Google colab: https://drive.google.com/drive/folders/1Ym_x3cKsYC2PhdwhtcOHqvNaKzdKIr_v?usp=sharing
5. Run these code blocks in order: first code block, code block 3-22 (ignore weird debugging output, its kinda out of order sorry), run the code block after this line (!zip -r model_backup.zip, ctrl f to find it)

Explanation for code: basically it includes an initial training and the first stage of fine-tuning for the model above but now I want to do a second stage of fine-tuning, which is why you would run the code block after this line (!zip -r model_backup.zip, ctrl f to find it). You need to upload the files in my Google Drive folder link above because it contains the model history for the initial training and first stage fine-tuning so that you don't have to rerun it again yourself. please let the groupchat know if you have any questions, thank you! 

## Data Exploration
### Dataset Description
The dataset is a subset of the FitzPatrick17k dataset, consisting of approximately 4,500 labeled images representing 21 skin conditions. The dataset includes:
* Images: Contained in images.zip, divided into train and test sets.
* Metadata: Provided in train.csv and test.csv, including the following key columns:
* md5hash: Unique identifier for each image.
* fitzpatrick_scale: Self-reported skin tone using the FitzPatrick Skin Type (FST) scale, ranging from 1 (light) to 6 (dark).
* fitzpatrick_centaur: Skin tone rating assigned by Centaur Labs, a medical data annotation firm.
* label: The diagnosed skin condition, serving as the target variable.
* qc: Quality control checks by board-certified dermatologists (available for a limited subset).
* ddi_scale: A column used for dataset reconciliation (not relevant to the competition task).

The dataset contains 21 distinct skin conditions, with clear class imbalances. Conditions such as acne vulgaris are overrepresented, while rare conditions have fewer samples, which could impact model performance.


Skin Tone Representation

The FitzPatrick scale ranges from 1 (light) to 6 (dark). The distribution reveals an underrepresentation of darker skin tones, highlighting the need for fairness-aware modeling techniques.

### Preprocessing Approaches
To prepare the data for model training, the following preprocessing techniques were applied:
* Label encoding
* Explore missing data
* Resizing and normalization
* Train-test-split
* preprocess_input: a preprocessing function specific to the EfficientNet model. It scales the pixel values from \[0, 255\] to the range \[-1, 1\] by applying the following transformation: preprocessed = image / 127.5 − 1
* ImageDataGenerator handles image preprocessing and augmentation. Augmentation techniques applied:
    * rotation_range=30: Randomly rotates images by up to 30 degrees.
    * width_shift_range=0.2: Shifts the image horizontally by up to 20% of the width.
    * height_shift_range=0.2: Shifts the image vertically by up to 20% of the height.
    * shear_range=0.2: Applies shearing transformations.
    * zoom_range=0.3: Randomly zooms images by up to 30%.
    * horizontal_flip=True: Randomly flips images horizontally.
    * vertical_flip=False: Vertical flipping is disabled.
    * brightness_range=[0.6, 1.4]: Randomly adjusts the brightness between 60% and 140% of the original.
    * fill_mode="nearest": Fills missing pixels with the nearest valid pixel values during transformations.
* Custom data generator function create_generator: This function creates data generators from the train_data and val_data DataFrames. Parameters used include the following:
    * dataframe: The DataFrame containing file paths and labels.
    * x_col: Column with image file paths.
    * y_col: Column with encoded labels.
    * target_size=(128, 128): Resizes all images to 128x128 pixels.
    * batch_size=32: Loads 32 images per batch.
    * class_mode='sparse': Uses sparse categorical labels.
    * shuffle=True: Randomizes image order during training but keeps it fixed during validation.
* Class balancing with augmentation: The code ensures that each class contains exactly 400 images by randomly sampling existing images if the class has more than 400 images and augmenting images to increase the count if the class has fewer than 400 images. This technique balances the dataset, ensuring all classes have an equal number of samples, which prevents the model from being biased toward classes with more images.

The training generator uses augmentation. The validation generator uses only EfficientNet preprocessing without augmentation.\
Potential improvements can be class balancing

Applied oversampling techniques to underrepresented skin tones to reduce class imbalance effects.

### Visualizations from the Exploratory Data Analysis (EDA)
We don't have visualizations from our EDA yet.

## Team & Contribution
[Irene Deng](https://github.com/irened123)\
[Athena Bai](https://github.com/athena-bai)\
[Mina Shimada](https://github.com/minashim)\
[Danaid Sinani](https://github.com/mrsinani)\
[Valentina Haddad](https://github.com/Valentina-Haddad25)\
[JinYu (Dora) Li](https://github.com/Dorajyl)\

