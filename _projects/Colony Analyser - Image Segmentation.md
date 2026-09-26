---
layout: page
title: Colony Analyser - Image Segmentation
description: A deep-learning web app for measuring fungal colonies from plate images.
img: assets/img/fv_colonies.webp
importance: 2
category: software
---

<br>

Measuring fungal colony growth by hand is slow and inconsistent. Colony Analyser replaces it with a segmentation model that outlines each colony in a plate photograph and reports its size and shape.

<br>

The model is a TransUNet (ViT/ResNet-50 hybrid), trained on HPC A100 GPUs and versioned on Hugging Face. It is served as a Dockerised web app with CPU-only inference. I first ran it on my own hardware; it now runs on NIAB infrastructure and is used by research groups to measure their plates.
