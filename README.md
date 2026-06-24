🏥 Real-Time Patient Monitoring System (Kafka + AI + Notifications)
🚀 Overview
This project is a real-time patient monitoring and alert system designed to help families stay informed about the condition of their loved ones admitted in hospitals.
Modern hospitals generate massive streams of patient vitals such as:

❤️ Heart Rate
🫁 SpO2 (Oxygen Levels)
📉 ECG signals

This system processes those streams using event-driven architecture and enhances them with AI-driven insights, delivering meaningful updates via:
✅ Telegram
✅ WhatsApp
✅ PDF Reports

🎯 Problem Statement
When a patient is in critical condition:

Family members constantly worry 😟
They can’t always be physically present
Doctors are busy and updates are delayed

👉 This system solves that by providing:
✅ Real-time monitoring
✅ Automated alerts
✅ AI-based health insights

🧠 Key Features
✅ Real-Time Streaming (Kafka)

Continuous ingestion of patient vitals
Event-driven architecture using Confluent Cloud


✅ Multi-Metric Analysis

Combines:

Heart Rate
SpO2


Provides context-aware health status


✅ Risk Score Calculation

Generates a score (0–100)
Based on abnormal readings

Example:
Risk Score: 78/100 → High Risk


✅ Trend Detection
Detect patient condition changes:

✅ Improving
⚠️ Worsening
🟡 Stable


✅ AI (LLM-Based Insights)

Converts raw data into:

Doctor-style summaries
Simple explanation for families



Example:
Patient condition is deteriorating due to low oxygen levels and elevated heart rate.
Immediate medical attention is recommended.


✅ PDF Report Generation
Includes:

Charts 📊
Risk score
AI summary
Vitals history


✅ Notifications
📱 Telegram Bot

Instant alerts (free & easy)

💬 WhatsApp (Twilio)

Real-world messaging experience


🧱 Architecture
                ┌──────────────┐
                │   Producer   │
                │(Dummy Data)  │
                └──────┬───────┘
                       ↓
              ┌─────────────────┐
              │ Confluent Kafka │
              │  (Topics)       │
              └──────┬──────────┘
                     ↓
            ┌──────────────────┐
            │   Processor      │
            │(Aggregation)     │
            └──────┬───────────┘
                   ↓
            ┌──────────────────┐
            │   AI Service     │
            │ Risk + LLM       │
            └──────┬───────────┘
                   ↓
       ┌───────────┴────────────┐
       ↓                        ↓
┌──────────────┐         ┌──────────────┐
│ PDF Generator│         │ Notifier     │
└──────────────┘         ├──────────────┤
                         │ Telegram     │
                         │ WhatsApp     │
                         └──────────────┘


📦 Kafka Topics


TopicDescriptionpatient-vitalsRaw incoming patient datapatient-ai-outputProcessed + AI insights

⚙️ Tech Stack

LayerTechnologyStreamingConfluent Cloud (Kafka)Processing.NET (C#)AILLM (Azure OpenAI / OpenAI)ChartsScottPlotPDFQuestPDFMessagingTelegram Bot, Twilio WhatsAppDeploymentDocker, AWS

🏗️ Project Structure
patient-monitoring-app/
│
├── producer/        # Simulates hospital devices
├── processor/       # Aggregates & filters data
├── ai-service/      # AI insights + risk + trend
├── notifier/        # Sends Telegram & WhatsApp alerts
│
├── README.md


🔧 Setup Instructions
✅ 1. Create Kafka Topics (Confluent Cloud)
patient-vitals
patient-ai-output


✅ 2. Update Credentials
In all services:
C#BootstrapServers = "<BOOTSTRAP_SERVER>"SaslUsername = "<API_KEY>"SaslPassword = "<API_SECRET>"Show more lines

✅ 3. Run Services (Important Order)
Terminal 1:
Shelldotnet run --project producerShow more lines
Terminal 2:
Shelldotnet run --project processorShow more lines
Terminal 3:
Shelldotnet run --project ai-serviceShow more lines
Terminal 4:
Shelldotnet run --project notifierShow more lines

📲 Notification Setup
✅ Telegram

Create bot via BotFather
Get:

BOT_TOKEN
CHAT_ID




✅ WhatsApp (Twilio)

Create Twilio account
Enable WhatsApp Sandbox
Add credentials:

ACCOUNT_SID
AUTH_TOKEN


📊 Sample Output
🚨 Patient Alert

Patient: P1
Risk Score: 82/100
Trend: Worsening ⚠️

Analysis:
Low oxygen level detected with high heart rate.

Doctor Summary:
Patient condition is critical and requires immediate attention.


🧠 Flink (Stream Processing Query)
SQLINSERT INTO patient_ai_outputSELECT    patientId,    AVG(heartRate),    AVG(spo2),    LEAST(100,        SUM(CASE WHEN heartRate > 120 OR heartRate < 50 THEN 5 ELSE 0 END) +        SUM(CASE WHEN spo2 < 90 THEN 10 ELSE 0 END)    ),    CASE        WHEN AVG(heartRate) > 120 AND AVG(spo2) < 90 THEN 'CRITICAL'        ELSE 'STABLE'    END,    window_start,    window_endFROM TABLE(    TUMBLE(TABLE patient_vitals, DESCRIPTOR(ts), INTERVAL '1' MINUTE))GROUP BY patientId, window_start, window_end;Show more lines

🚀 Future Enhancements

📱 Mobile app for family members
📊 Real-time dashboard UI
🤖 AI prediction (future risk)
☁️ Full AWS deployment (ECS + SNS + S3)
🏥 Integration with hospital systems


🏆 Hackathon Pitch

“We built a real-time patient monitoring system that combines Kafka streaming with AI to convert raw medical data into meaningful insights and instantly notify families via WhatsApp and Telegram.”


❤️ Impact

Reduces anxiety for families
Improves communication transparency
Enables proactive healthcare decisions


👨‍💻 Author
Shahrukh Alam
Sr Full Stack Engineer
