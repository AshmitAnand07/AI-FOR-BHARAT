🌾 Krishi Jeevan
Financial Intelligence Training for Farmers – Delivered as a Voice-First Game
🚀 One-Line Summary

A voice-first, offline financial life simulator that trains farmers to manage seasonal income, choose safe credit, build savings, and protect against financial risk — through experiential gameplay.

🎯 Problem Statement

Small and marginal farmers in rural India face:

Seasonal income volatility

High-interest informal lending

Low financial literacy

Financial shocks (health, crop loss, price crash)

Limited digital access and poor connectivity

Traditional financial literacy tools fail because they are abstract, lecture-based, and not adapted to rural realities.

💡 Our Solution

Krishi Jeevan transforms financial education into an interactive farming-season simulation.

Instead of teaching theory, it allows farmers to:

Experience real financial consequences safely

Compare formal vs informal loan systems

Understand interest accumulation

Learn savings discipline

See how insurance protects against risk

All delivered in a voice-first, offline-capable mobile app optimized for low-end Android devices.

🧠 What Makes It Unique

🎙 Voice-first interaction for low-literacy users

📱 Fully offline-first architecture

💰 Realistic loan & compound interest modeling

⚠️ Risk simulation with probability logic

🔄 Sync when internet becomes available

🌍 Multi-language support

🧩 Modular and scalable system design

This is not a quiz app — it is a behavioral financial simulator.

🏗 High-Level Architecture

The system follows a layered architecture:

Presentation Layer – Voice UI & Touch UI

Simulation Engine – Financial & Risk Logic

Business Logic Layer – Loan, Savings, Interest Calculations

Data Layer – Encrypted Local Database + Sync Queue

Backend Services – Analytics, Content, Authentication

Full architecture available in:
📄 design.md

📚 Documentation

📄 Requirements Document

📄 Technical Design Document

Both documents are structured for hackathon evaluation.

🛠 Tech Stack
Mobile App

Kotlin (Native Android)

Jetpack Compose

Room (SQLite)

WorkManager

On-device ML Kit (Speech-to-Text)

Android TTS

Backend

Node.js + Express

PostgreSQL

Redis

AWS (ECS, RDS, S3, CloudFront)

Security

AES-256 encryption

TLS 1.3

JWT authentication

Android Keystore

📶 Offline-First Design

All gameplay logic runs locally

Financial calculations execute on-device

Data stored securely in encrypted local DB

Sync queue ensures eventual consistency

No dependency on constant internet

Designed specifically for rural Bharat connectivity conditions.

📊 Impact Vision

Krishi Jeevan aims to:

Improve farmer financial decision-making

Reduce dependency on predatory lending

Encourage savings and insurance adoption

Build financial confidence through simulation

Target: 1M+ farmers

🔮 Future Scope

Multi-language expansion

AI-based personalization engine

Government scheme integration

NGO partnership deployment

WhatsApp & USSD integration

iOS version

⚠️ Disclaimer

Krishi Jeevan is a financial training simulation and does not provide real financial advisory services. It does not connect to actual banking systems in the MVP phase.
