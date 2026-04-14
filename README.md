1. Paper Overview

The paper studies security vulnerabilities in machine learning systems that use Multi-Exit Networks (MEN). These networks are designed to improve inference efficiency by allowing predictions to exit early in the model when the prediction confidence is high.

However, the paper shows that attackers can exploit this architecture through a type of attack called a sponge attack. The goal of the attack is not necessarily to cause wrong predictions, but to increase the computational cost and latency of the model.

The attack forces most inputs to bypass early exits and travel through deeper layers of the network. This increases resource consumption and can degrade the performance of systems that rely on efficient inference.

2. Multi-Exit Networks (MEN)

A Multi-Exit Network is a neural network architecture that contains multiple exit points at different depths of the network.

In a traditional neural network, an input must pass through all layers before producing an output. In contrast, MEN allows predictions to be made earlier in the network when the model is confident enough.

This design improves efficiency because:

simple inputs can exit early
complex inputs can continue deeper into the network
overall inference time is reduced

The structure of a MEN can be represented conceptually as:

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

If the model has high confidence at an early exit, the prediction is returned immediately. Otherwise, the input continues to deeper layers.

3. Early Exit Mechanism

The early exit mechanism is based on a confidence threshold.

At each exit point:

The model produces a prediction.
The confidence of the prediction is evaluated.
If the confidence is above a threshold, the prediction is returned.
Otherwise, the input continues to deeper layers.

This mechanism helps reduce computational cost because many inputs do not need to pass through the entire network.

4. Sponge Attack

A sponge attack is designed to increase the computational cost of inference.

Instead of causing incorrect predictions, the attacker creates inputs that force the model to process the input through deeper layers of the network.

In other words, the attack attempts to avoid early exits.

As a result:

the model performs more computations
inference becomes slower
system resources are consumed unnecessarily

This can reduce the efficiency benefits provided by Multi-Exit Networks.

5. Data Poisoning

Data poisoning is another important concept related to the attack.

In data poisoning, an attacker modifies the training dataset by inserting specially crafted samples. These poisoned samples influence the behavior of the trained model.

When poisoning targets Multi-Exit Networks, it can cause the model to:

reduce early exit usage
process more inputs through deeper layers
increase computational cost during inference

This makes sponge attacks more effective.

6. Key Metrics Used in the Paper
Exit Rate

Exit rate refers to the percentage of samples exiting at each exit level of the network.

For example:

Exit 1 → 60% of inputs
Exit 2 → 25% of inputs
Exit 3 → 15% of inputs

A high early exit rate usually indicates efficient model performance.

During a sponge attack, the early exit rate decreases because more inputs are forced to continue deeper into the network.

Latency

Latency refers to the time required to process an input and produce a prediction.

When inputs exit early, latency is low. However, if the inputs travel through deeper layers, latency increases.

Sponge attacks increase latency because they force inputs to bypass early exits and reach deeper layers.

7. Impact of Sponge Attacks

Sponge attacks can negatively affect machine learning systems in several ways:

increased computational cost
slower inference time
higher energy consumption
reduced efficiency of deployed ML models

In large-scale systems, this type of attack could potentially lead to resource exhaustion or denial-of-service conditions.

8. Application in This Project

This project will simulate the concepts described in the paper.

The system will include:

a Python module that simulates a Multi-Exit Network
generation of sponge attack inputs
measurement of metrics such as exit level and latency

The generated data will be sent to a Spring Boot backend, which will:

receive the metrics through an API
run a detection mechanism
store the results in a database for analysis

This setup allows experimentation with how sponge attacks affect Multi-Exit Networks and how such attacks might be detected.
