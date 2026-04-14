# System Design

This document describes the system architecture and design for detecting sponge attacks in Multi-Exit Networks.  
The system is based on the ideas presented in the paper **"Sponge Attack Against Multi-Exit Networks With Data Poisoning"**.

The project simulates a Multi-Exit Network, generates attack scenarios, produces metrics, and analyzes those metrics using a backend detection system.

---

## 1. Project Overview

The goal of this project is to simulate a **Multi-Exit Network (MEN)** environment and study how sponge attacks affect the efficiency of the network.

The system generates metrics such as **exit level and latency** during inference and analyzes them to detect abnormal behavior.

The architecture consists of three main parts:

- Python simulation environment
- Message queue communication layer
- Java Spring Boot backend with database storage

The Python system simulates the network and generates metrics.  
These metrics are sent through a **message queue** to the backend detection system for analysis.

---

## 2. System Goals

The system aims to achieve the following objectives:

- Simulate a **Multi-Exit Network (MEN)** environment
- Generate **sponge attack scenarios**
- Produce metrics such as exit level and latency
- Transfer metrics using an **event-driven communication system**
- Detect abnormal patterns in the generated metrics
- Store the results for further analysis

---

## 3. High-Level System Architecture

The system follows an **event-driven architecture** where the simulator publishes metrics and the backend consumes them for analysis.

### Architecture Diagram


Python MEN Simulator
↓
Metrics Generator
↓
Message Queue
↓
Spring Boot Consumer
↓
Detection Engine
↓
Database
↓
Result API


This architecture decouples the simulation component from the detection system, allowing asynchronous data processing.

---

## 4. System Components

### 4.1 Python MEN Simulator

The Python module is responsible for simulating the behavior of a Multi-Exit Network.

Responsibilities:

- Simulate MEN inference behavior
- Generate sponge attack samples
- Calculate metrics such as exit level and latency
- Publish metrics to the message queue

---

### 4.2 Message Queue

The message queue acts as the communication layer between the simulator and the backend system.

Responsibilities:

- Receive metrics from the Python producer
- Temporarily store messages
- Deliver messages to backend consumers
- Enable asynchronous processing

Possible technologies:

- Apache Kafka
- RabbitMQ

---

### 4.3 Spring Boot Consumer

The Spring Boot service consumes messages from the queue.

Responsibilities:

- Subscribe to the message queue
- Receive incoming metrics
- Forward metrics to the detection engine
- Store processed data in the database

---

### 4.4 Detection Engine

The detection engine analyzes incoming metrics to identify abnormal behavior.

Responsibilities:

- analyze latency values
- detect abnormal exit patterns
- identify potential sponge attack indicators

The detection logic is based on analyzing metrics produced during the simulation.

---

### 4.5 Database

The database stores all generated metrics and analysis results.

Responsibilities:

- store sample metrics
- store detection results
- support analysis queries

---

## 5. Data Flow

The system processes data using an **event-driven pipeline**.

Step-by-step process:

1. The Python simulator runs the MEN model.
2. Metrics such as exit level and latency are generated.
3. The metrics are converted into JSON format.
4. The JSON message is published to the message queue.
5. The Spring Boot consumer reads the message from the queue.
6. The detection engine analyzes the metrics.
7. The processed results are stored in the database.
8. Results can be retrieved using backend APIs.

---

## 6. Message Format (JSON Schema)

Metrics are transmitted as JSON messages.

Example format:

```json
{
  "exit": 1,
  "latency": 50,
  "trigger": false,
  "timestamp": "2026-04-14T10:00:00"
}

```

---

## 7. Database Design

The system stores generated metrics in a database table called **samples**.

### Table: samples

| Column | Description |
|------|-------------|
| id | unique identifier |
| exit_level | exit used by the sample |
| latency | inference time |
| trigger | attack indicator |
| timestamp | sample creation time |

---

## 8. Detection Logic (Conceptual)

The detection engine analyzes incoming metrics to identify abnormal behavior that may indicate a sponge attack.

### Possible Detection Indicators

- unusually high latency
- abnormal exit distribution
- repeated triggered samples

### Detection Outcome

If suspicious behavior is detected, the system flags the sample as a **potential attack instance**.  
These flagged samples can then be stored and analyzed further.

---

## 9. Result APIs

The backend provides APIs to access stored metrics and detection results.

### Example Endpoints

#### GET /api/results

Returns detection results and analysis.

#### GET /api/samples

Returns stored sample metrics.
