# Multimodal Embeddings based on OVMS

**Google Summer of Code 2025 – Intel-OpenVINO toolkit**

---

## 📑 Table of Contents
1. [Overview](#-overview)
2. [Objectives](#-objectives)
3. [Features](#-features)
4. [Tech Stack](#-tech-stack)
5. [Repository Structure](#-repository-structure)
6. [Installation](#-installation)
7. [Usage](#-usage)
8. [GSoC Contributions](#-gsoc-contributions)
9. [Demo](#-demo)
10. [Acknowledgements](#-acknowledgements)
11. [License](#license)

---

## 📌 Overview
This project implements a **multimodal embeddings pipeline** using the **OpenVINO Model Server (OVMS)** to efficiently generate and serve embeddings for multiple data modalities, such as **text** and **images**, in a unified vector space.

It enables developers to integrate **semantic understanding** into their applications for tasks like **semantic search**, **recommendation systems**, and **cross-modal retrieval**, while leveraging OVMS for **optimized inference on Intel hardware**.

---

## 🎯 Objectives
- Deploy and serve **multimodal embedding models** via OVMS.
- Provide a **single API endpoint** for embeddings from multiple modalities.
- Support **real-time inference** and **batch processing**.
- Demonstrate integration into **downstream AI applications**.

---

## 🚀 Features
- **Unified embedding space** for text and image modalities.
- **OVMS-powered model serving** for speed and scalability.
- **REST and gRPC API** support.
- **Preprocessing pipeline** for different input types.
- **Batch inference** for improved throughput.

---

## 🛠 Tech Stack
- **Languages:** Python, C++
- **Transformers:**Hugging Face
- **Model Serving:** OpenVINO Model Server (OVMS)
- **Frameworks:** PyTorch / TensorFlow (depending on model)
- **APIs:** REST, gRPC
- **Deployment:** Docker, Kubernetes (if applicable)

---

## 📂 Repository Structure




