# 🛄 Smart Baggage Reclaim Management System

An intelligent, event-driven baggage belt management system for airports, built with **Spring Boot**, **Apache Kafka**, and **PostgreSQL**. The system automates belt assignment based on real-time flight updates, delays, and maintenance events.

[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://openjdk.org/projects/jdk/21/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.2-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Apache Kafka](https://img.shields.io/badge/Apache%20Kafka-3.5+-red.svg)](https://kafka.apache.org/)
[![Docker](https://img.shields.io/badge/Docker-24.0+-blue.svg)](https://www.docker.com/)

---

## ✈️ Project Overview

Airports need to manage complex baggage routing in real time. This system ensures:
- **Smart belt assignment** for incoming flights.
- **Kafka-powered event handling** for flight status updates (e.g., delays, landings, cancellations).
- **Dynamic belt reassignment** during emergencies or maintenance.
- **Efficient resource utilization** and predictive belt availability.

---

## 🛠 Tech Stack

| Technology        | Description                               |
|-------------------|-------------------------------------------|
| **Spring Boot**   | Backend framework for REST API & services |
| **Apache Kafka**  | Event streaming for flight & belt updates |
| **PostgreSQL**    | Persistent storage for flights & belts    |
| **Lombok**        | Reduces boilerplate code                  |
| **JPA/Hibernate** | ORM for database interaction              |
| **KafkaTemplate** | Kafka producer integration                |

---

## 🧠 Core Features

###  Flight Module
- Track flights and status (`SCHEDULED`, `LANDED`, `DELAYED`, `CANCELLED`)
- Kafka producer publishes updates like:
  - Arrival
  - Delay
  - Gate change
  - Cancellation

###  Baggage Belt Module
- Assign best-suited belt for flights (based on capacity, distance, availability)
- Automatically reassign belts on delays or maintenance
- Predict upcoming availability
- Mark belts as **MAINTENANCE**, notify via Kafka

###  Kafka Events
- `flight-updates` topic for flight events
- `belt-maintenance` topic for belt status
- Kafka consumers dynamically react to events and trigger logic (e.g., assign or reassign belts)

---

## 🔄 System Flow

```mermaid
sequenceDiagram
    participant FlightController
    participant KafkaProducer
    participant KafkaConsumer
    participant BeltAssignmentService
    participant BaggageBelt

    FlightController->>KafkaProducer: Publish flight update
    KafkaProducer-->>KafkaTopic: Send FlightUpdateMessage
    KafkaConsumer-->>BeltAssignmentService: On ARRIVAL -> assignBeltToFlight()
    KafkaConsumer-->>BeltAssignmentService: On DELAY -> reassignBeltDueToDelay()
    BeltAssignmentService-->>BaggageBelt: Update status





sequenceDiagram
    autonumber
    participant 🧑‍✈️ FlightController as <b>FlightController</b><br><img src="https://img.icons8.com/fluency/48/spring-logo.png" width="20"/> Spring Boot
    participant 📤 KafkaProducer as <b>KafkaProducer</b><br><img src="https://img.icons8.com/ios-filled/50/kafka.png" width="20"/> Kafka
    participant 📨 KafkaConsumer as <b>KafkaConsumer</b><br><img src="https://img.icons8.com/ios-filled/50/kafka.png" width="20"/> Kafka
    participant 🛠️ BeltAssignmentService as <b>BeltAssignmentService</b><br><img src="https://img.icons8.com/fluency/48/spring-logo.png" width="20"/> Spring Boot
    participant 🛄 BaggageBelt as <b>BaggageBelt</b><br><img src="https://cdn-icons-png.flaticon.com/512/919/919853.png" width="20"/> PostgreSQL

    🧑‍✈️ FlightController->>📤 KafkaProducer: Publish flight update
    📤 KafkaProducer-->>KafkaTopic: Send <code>FlightUpdateMessage</code>
    📨 KafkaConsumer-->>🛠️ BeltAssignmentService: On ARRIVAL → assignBeltToFlight()
    📨 KafkaConsumer-->>🛠️ BeltAssignmentService: On DELAY → reassignBeltDueToDelay()
    🛠️ BeltAssignmentService-->>🛄 BaggageBelt: Update belt status

