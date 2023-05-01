# Federated Learning on FVM - TechieTeee

<https://youtube.com/watch?v=CZDIFUMACM8>

![Federated Learning on FVM - TechieTeee](/thing23/CZDIFUMACM8.jpg)

## Overview

In this video, TechieTeee introduces federated machine learning on FVM (Filecoin Virtual Machine). The presentation covers the process of creating a federated learning model using the Flower library, exporting the model into ONNX format, and converting it into bytecode for deployment on a smart contract.

## Video Content

### Introducing Federated Machine Learning (FML)

FML is a new approach in which the model is sent to the device instead of the data being sent to the model. This allows for more data privacy for the user and expands the use cases in highly regulated industries, such as law and medicine, where data privacy is a critical concern.

### Creating a Federated Machine Learning Model

TechieTeee demonstrates how to create a federated machine learning model using Google Colab and the Flower library. The dataset used in this example is CIFAR10, which consists of 60,000 images in 10 object classes. By setting up the federated learning environment, the model can learn from 16 separate devices without having to share each device's personal data.

### Exporting and Deploying the Model

Once the model is created and trained, it needs to be exported into ONNX format and converted into bytecode. This process makes the model understandable for smart contracts. TechieTeee provides a sample smart contract for deploying the model and demonstrates how to use it.

## Key Takeaways

- Federated machine learning allows for more data privacy and expands use cases in highly regulated industries.
- The Flower library is an easy-to-use tool for creating federated machine learning models in Python.
- The ONNX format allows machine learning models to be understandable for smart contracts and blockchain deployment.