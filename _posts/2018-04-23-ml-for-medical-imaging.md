---
layout: post
title: Machine learning for medical imaging 
tags: research
---

# Machine learning (ML)
-
- ML has three broad categories: supervised learning, unsupervised learning, and reinforcement learning.

	1. Supervised learning
		- you give the algorithm labeled data, meaning input features (e.g. cystoscopy image from a patient) with paired labels (that same patient's comorbidities, length of stay, infection, hematuria, pain, etc.))
		- The algorithm "learns" or maps the associations between the features and the labels.
		- A good algorithm can generalize in the sense that it can see new features from new patients and then correctly predict what the labels (outcomes) will be.
		- See https://www.youtube.com/watch?v=bQI5uDxrFfA for a great summary by one of the leaders in AI.

	2. Unsupervised learning
		- For evaluating hidden structure in data that do not have labels, e.g. clustering your data and seeing if the clusters have clinical meaning.
		- See https://www.youtube.com/watch?v=Ev8YbxPu_bQ

	3. Reinforcement learning
		- how software agents ought to take actions in an environment so as to maximize some notion of cumulative reward.
		- This is partially how self-driving cars and Google's AlphaGo works.
		- Has been used to optimize heparin dosing guidelines and maintained therapeutic INR levels.
		- Review paper of these concepts applied to medical imaging: Erickson et al. Machine Learning for Medical Imaging. RadioGraphics 2017 37:2, 505-51

# Deep learning (DL)
- Fancy subfield that has been applied to all areas of ML, and is the current mainstay of AI research.
- Describes the architecture of the algorithm.
- Performance scales with data, e.g. if you give a DL algorithm 10,000 cystoscopy images it will get super accurate, whereas a non-DL algorithm might top out and not get any better past a few hundred images.
- Focus on stuff above and below before diving in to DL.

# Applications in clinical imaging 
- Diabetic retinopathy: the Google paper you've seen
- Cardiac MRI: Arterys, a medical imaging technology company, recently partnered with GE Healthcare's cardiac MRI technology to better visualize and quantify blood flow inside the heart and dramatically speed up image processing.
- Dermatology: from Stanford and Google reports the use of deep learning analysis of dermoscope images to distinguish 1) keratinocyte carcinomas versus benign seborrheic keratoses; and 2) malignant melanomas versus benign nevi.
- Glioma: paper attached; work done by Emory researchers I know (one of whom now chairs Northwestern pathology) reports prediction of overall survival of patients diagnosed with brain tumors from microscopic images of tissue biopsies and genomic biomarkers.
- Consolidation in chest X-rays

# Resources to learn the basics
- Python 3 (do not learn Python 2)
- 
