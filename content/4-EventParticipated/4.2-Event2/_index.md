---
title: "COMMUNITY DAY"
date: "2026-05-23"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---


# Summary Report: “AWS COMMUNITY DAY”

### Event Objectives

- Provide a practical perspective on optimizing performance, infrastructure cost, and applying AI/ML to solve business problems.
- Introduce no-code automation tools (Amazon Quick) and multi-agent system design (Multi-Agent).
- Share hands-on experience from major Hackathon competitions and deep analysis of how Large Language Models (LLMs) actually operate.
- Create strong networking opportunities between developers, students, and top technology experts within the AWS ecosystem.

---

### Agenda Overview

**Time:** 8:30 AM – 12:00 PM, Saturday, May 23, 2026  
**Location:** AWS Vietnam Office, 36th Floor  

---

## Key Highlights

### 1. Welcome & Check-in (8:30 – 9:00 AM)

- Participants checked in, settled in on the 36th floor, and freely connected with each other for extended networking.

---

### 2. Session 1: Context Is Everything: Making AI Actually Work for You (09:00 – 09:30 AM)

- **Speaker:** Tinh Truong (Anh Tinh) – Platform Engineer, GoTymeX
- **Core topics:**

- Why AI responses fail when context is missing and how to define “context” properly.
- The evolution of AI from prompt writing to memory management systems (the concept of a second AI brain).
- Practical thinking methods and tips for leveraging context to achieve optimal outcomes.
- Career direction and a roadmap for students to start building AI applications, followed by a Q&A session.

---

### 3. Session 2: Friendly AI Assistant with Amazon Quick (09:30 – 09:45 AM)

- **Speaker:** Pham Ng Hai Anh (Hai Anh) – AWS Community Builder, G-AsiaPacific Vietnam
- **Core topics:**

- **Quick Chat Agent:** An AI assistant for exploring data and analyzing specialized industry information.
- **Quick Flows:** Automating intelligent processes directly with natural language, without coding.
- **Quick Spaces:** Collaborative workspaces that transform personal information into team-wide structured knowledge.
- **Quick Sight:** Quickly building charts and reports from raw data through natural language commands.

---

### 4. Session 3: From Edge To Origin: CloudFront as Your Foundation (09:45 – 10:25 AM)

- **Speaker:** Nguyen Tuan Thinh (Thinh) – DevOps Engineer, First Cloud AI Journey
- **Core topics:**

- The flexible adaptability of Amazon CloudFront to all types of workloads.
- Cost optimization and bandwidth resource savings through the CloudFront network.
- Advanced security features protecting the edge (mitigating DDoS attacks, origin cloaking, and TLS management).
- Enhancing high availability with origin failover and accelerating global delivery performance.

---

### 5. Session 4: 36 hrs with LotusHacks – Building UTMorpho from Idea to Reality (10:25 – 10:55 AM)

- **Speaker:** Team VIB
- **Core topics:**

- The motivation to join LotusHacks — one of Vietnam’s largest Hackathon platforms.
- The journey from zero to an initial idea.
- Shaping the real problem and building the product framework for UTMorpho.
- The intense 36-hour development experience under pressure.
- Lessons from obstacles and technical issues (token limit overrun, AI overgeneration) and the turning point that changed the game.
- System architecture overview and live demo of the UTMorpho product.

---

### 6. Coffee Break (10:55 – 11:00 AM)

- A short break to enjoy refreshments and freely connect with AWS mentors.

---

### 7. Session 5: Non-Determinism of "Deterministic" LLM Settings (11:00 – 11:30 AM)

- **Speaker:** Duc Dao (Dao Duc) – Solution Architect, Cloud Kinetics
- **Core topics:**

- The token selection mechanism in language models (logits, softmax, and temperature).
- Examining the common assumption: does `Temperature = 0` truly guarantee deterministic output?
- The reality: why API inference optimization and floating-point math on GPUs can still produce result variance.
- Practical consequences for production applications that require high accuracy.
- Error mitigation strategies (majority voting, enforcing structured output, and adjusting repetition penalties).

---

### 8. Session 6: Enterprise-Grade Multi-Agent System: The Case of Startup Credit Scoring (11:30 AM – 12:00 PM)

- **Speaker:** Cat Vy
- **Core topics:**

- The core data structure mismatch between traditional banking systems and internal startup information.
- Single Agent: when to use it and when it becomes overloaded.
- The trend toward multi-agent architecture.
- The system design of a virtual credit committee.
- Establishing behavioral guardrails and business compliance.
- Measuring operational ROI and the roadmap for real-world deployment, with a closing Q&A.

---

## Key Takeaways

- **Context matters more than prompts:** AI cannot read your mind. Poor AI output usually comes from vague or noisy input context. Providing clear goals, evidence, and constraints is the key to making AI work effectively.
- **Optimize cost and performance at the edge:** Using Amazon CloudFront eliminates the risk of sudden cost spikes from fake traffic or DDoS attacks.
- **The truth about Temperature = 0:** This setting does not guarantee 100% identical outputs. Due to GPU computation variance and request batching.
- **Big ideas come from small pains:** Under tight deadlines, team alignment and a lean demo-worthy idea are more important than trying to build too many features.
- **The agentic shift:** Technology is strongly moving toward no-code automation via natural language (Amazon Quick) and multi-agent coordination to solve complex business problems, rather than relying on a single agent.
