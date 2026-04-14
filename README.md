
## 1. Paper Overview

The paper studies security vulnerabilities in machine learning systems that use **Multi-Exit Networks (MEN)**.

Multi-Exit Networks are designed to improve inference efficiency by allowing predictions to exit early in the model when the prediction confidence is high.

However, the paper shows that attackers can exploit this architecture using a type of attack called a **Sponge Attack**.

### Main idea of the attack

The goal of the sponge attack is **not to change the prediction result**, but to:

- increase computational cost
- increase inference latency
- reduce system efficiency

The attack forces most inputs to **bypass early exits and travel deeper into the network**, which increases resource consumption.

---

## 2. Multi-Exit Networks (MEN)

A **Multi-Exit Network** is a neural network architecture that contains **multiple exit points at different depths** of the model.

In a traditional neural network:


Input → Layers → Final Prediction


The input must pass through **all layers** before producing a prediction.

In contrast, a **Multi-Exit Network** allows predictions to exit earlier if the model is confident.

### Benefits of MEN

- Simple inputs can exit early
- Complex inputs continue deeper into the network
- Overall inference time is reduced
- Computational resources are used more efficiently

### Conceptual Structure of MEN


Input
↓
Layer 1
↓
Exit 1
↓
Layer 2
↓
Exit 2
↓
Layer 3
↓
Final Exit


If the model has high confidence at an early exit, the prediction is returned immediately.  
Otherwise, the input continues to deeper layers.

---

## 3. Early Exit Mechanism

The **early exit mechanism** works based on a **confidence threshold**.

At each exit point:

1. The model produces a prediction.
2. The confidence score of the prediction is calculated.
3. If the confidence is **greater than a threshold**, the prediction is returned.
4. Otherwise, the input continues to deeper layers.

### Why this is useful

This mechanism helps reduce computational cost because many inputs do not need to pass through the entire network.

---

## 4. Sponge Attack

A **Sponge Attack** is designed to **increase the computational cost of inference**.

Instead of causing incorrect predictions, the attacker generates inputs that force the model to process them through **deeper layers of the network**.

In simple terms, the attack tries to **avoid early exits**.

### Result of the attack

- The model performs more computations
- Inference becomes slower
- System resources are consumed unnecessarily

This reduces the efficiency advantages provided by Multi-Exit Networks.

---

## 5. Data Poisoning

**Data poisoning** is another concept used in the attack.

In this scenario, an attacker modifies the **training dataset** by inserting specially crafted samples.

These poisoned samples influence the training process and change the behavior of the model.

### Effect on Multi-Exit Networks

Poisoned data can cause the model to:

- reduce early exit usage
- process more inputs through deeper layers
- increase computational cost during inference

As a result, sponge attacks become more effective.

---

## 6. Key Metrics Used in the Paper

### Exit Rate

Exit rate refers to the **percentage of samples exiting at each exit level** of the network.

Example:


Exit 1 → 60% of inputs
Exit 2 → 25% of inputs
Exit 3 → 15% of inputs


A **high early exit rate** usually indicates efficient model performance.

During a sponge attack, the **early exit rate decreases** because more inputs are forced to continue deeper into the network.

---

### Latency

Latency refers to the **time required to process an input and produce a prediction**.

- Early exits → lower latency
- Deeper layers → higher latency

Sponge attacks increase latency because they force inputs to bypass early exits.

---

## 7. Impact of Sponge Attacks

Sponge attacks can negatively affect machine learning systems in several ways:

- Increased computational cost
- Slower inference time
- Higher energy consumption
- Reduced efficiency of deployed ML models

In large-scale systems, this could potentially lead to **resource exhaustion or denial-of-service conditions**.

---

## 8. Application in This Project

This project aims to simulate the concepts described in the paper.

### Python Module

Responsible for:

- Simulating a **Multi-Exit Network**
- Generating **sponge attack inputs**
- Producing metrics such as:
  - exit level
  - latency

### Spring Boot Backend

Responsible for:

- Receiving metrics through an API
- Running a detection mechanism
- Storing results in a database

### Database

Used to store:

- sample data
- exit levels
- latency values
- attack indicators

### System Flow


Python MEN Simulator
↓
Metrics Generation
↓
Spring Boot API
↓
Detection Engine
↓
Database Storage
↓
Result API


This setup allows experimentation with how sponge attacks affect Multi-Exit Networks and how such attacks can be detected.
