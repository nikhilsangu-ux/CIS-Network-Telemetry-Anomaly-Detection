# Network Telemetry Anomaly Detection for IoT Device Fleet Health

> Spot silent device failures (disconnects and reboots) before they happen, using AWS IoT and machine learning.

**Course:** Cloud Infrastructure and Services (25CS2206E) | **A.Y.** 2026-2027
**Project ID:** P287 | **Domain:** Anomaly Detection across Cyber-Physical Systems

---

## About the Project

Imagine hundreds of small smart devices installed across a campus or a factory. Now and then one of them quietly disconnects or restarts, and nobody notices until something stops working.

This project watches the health of every device in the fleet and warns the administrator **before** a device fails. Each device sends simple health details to the cloud (signal strength, uptime and free memory). A machine learning model learns what "normal" looks like and flags anything unusual.

## Key Features

- Live health telemetry from ESP32 / Raspberry Pi devices
- Secure device connection using MQTT and X.509 certificates
- Automatic processing and storage of every message
- Anomaly detection model trained on normal device behaviour
- Instant alerts by email or SMS when a device looks unhealthy
- Dashboards and alarms for continuous monitoring
- Low cost, since it mostly uses AWS free-tier services

## How It Works

````
Sensors → ESP32 / Raspberry Pi → AWS IoT Core (MQTT, X.509) → IoT Rule
                                                                  │
                                                                  ▼
                                                             AWS Lambda
                                                                  │
                                                                  ▼
                                                          Amazon DynamoDB
                                                                  │
                           ┌──────────────────┬───────────────────┤
                           ▼                  ▼                   ▼
                  Amazon SageMaker AI    Amazon SNS          Amazon S3
                  (anomaly detection)    (alerts)         (data archive)

                    Everything is monitored with Amazon CloudWatch
````

1. **Device:** the ESP32 / Raspberry Pi measures RSSI (Wi-Fi signal strength), uptime and free heap memory.
2. **Connect:** the data is published securely to AWS IoT Core over MQTT.
3. **Route:** an IoT Rule sends each message to AWS Lambda.
4. **Store:** Lambda checks the data and saves it in Amazon DynamoDB.
5. **Detect:** a model on Amazon SageMaker AI spots warning signs such as a weakening signal or frequent restarts.
6. **Alert:** Amazon SNS notifies the administrator right away.
7. **Archive and monitor:** raw data goes to Amazon S3 for future training, and Amazon CloudWatch shows dashboards and alarms.

## AWS Services Used

| Service | Role in the project | Free-tier note |
|---|---|---|
| AWS IoT Core | Device connection, MQTT, X.509 authentication, IoT Rules | Free for 12 months |
| AWS Lambda | Processes incoming telemetry | Always free |
| Amazon DynamoDB | Stores device health records | Always free (25 GB) |
| Amazon SageMaker AI | Trains and runs the anomaly detection model | Free trial for 2 months, then paid |
| Amazon SNS | Sends alerts | Always free (monthly allowance) |
| Amazon S3 | Archives raw and historical data | Free for 12 months (5 GB) |
| Amazon CloudWatch | Metrics, logs, dashboards and alarms | Always free (basic metrics and alarms) |

## Hardware

- ESP32 or Raspberry Pi (controller)
- Wi-Fi connection

**Telemetry collected:** RSSI, uptime, free heap memory

## Repository Structure

````
.
├── device/          # ESP32 / Raspberry Pi code that publishes telemetry
├── lambda/          # Lambda function that processes and stores data
├── ml/              # Model training and anomaly detection scripts
├── infra/           # IoT Rule, DynamoDB, SNS and CloudWatch setup
├── docs/            # Abstract, architecture diagram, screenshots
└── README.md
````

## Getting Started

### Prerequisites

- An AWS account (free tier is enough to begin)
- ESP32 or Raspberry Pi
- Arduino IDE / PlatformIO (ESP32) or Python 3 (Raspberry Pi)

### Setup Steps

1. **Create a Thing** in AWS IoT Core and download its X.509 certificates.
2. **Flash the device** with the code in `device/` and add your certificates and endpoint.
3. **Create an IoT Rule** that forwards telemetry to the Lambda function.
4. **Create the DynamoDB table** to store the readings.
5. **Deploy the Lambda function** from `lambda/`.
6. **Set up the SNS topic** and subscribe your email.
7. **Train the model** in SageMaker AI using the scripts in `ml/`.
8. **Create CloudWatch dashboards and alarms** to monitor the fleet.

> Never commit your certificates, private keys or AWS credentials to GitHub. Add them to `.gitignore`.

## Expected Outcomes

- Early detection of failing devices
- Less downtime and faster troubleshooting
- A scalable, low-cost way to monitor a fleet of IoT devices

## Team 8

| Name | Roll No. |
|---|---|
| T. Hasini | 2520030102 |
| S. Nikhil | 2520030246 |
| CH. Pavan Kumar | 2520030312 |

**Guide:** Mrs. K Hema, Assistant Professor

---

*Submitted as part of the Cloud Infrastructure and Services course, A.Y. 2026-2027.*
````
````

The README itself contains code blocks, so the outer box above has four backticks. When you paste, copy only what's inside the box. The final line with three backticks before the closing fence is a display quirk: if it shows up at the very end of your pasted text, delete that last stray line.
