# REQUIREMENTS.md – **problem-validator**

---

## 1. Overview

`problem-validator` is an Axentx product that empowers entrepreneurs to validate their business ideas by:

1. Systematically identifying real customer problems.
2. Measuring willingness‑to‑pay (WTP) for potential solutions.

The solution will be a web‑based platform with a RESTful API, leveraging Axentx’s shared knowledge base (pgvector) and the production‑grade inference engine **vLLM** for AI‑driven insights.

---

## 2. Functional Requirements

| ID | Requirement | Description | Acceptance Criteria |
|----|-------------|-------------|---------------------|
| **FR‑1** | **User Authentication & Authorization** | Secure login using OAuth 2.0 (Google, GitHub, or Axentx SSO). Role‑based access: *Entrepreneur*, *Admin*. | • Successful login redirects to dashboard.<br>• Unauthorized users receive 401/403.<br>• Role changes propagate instantly. |
| **FR‑2** | **Idea Submission** | Entrepreneurs can create, edit, and delete “Idea” records with title, description, industry tags, and optional media. | • Form validates required fields.<br>• Duplicate titles within the same user are rejected.<br>• CRUD operations persist in PostgreSQL. |
| **FR‑3** | **Problem Identification Workflow** | AI‑driven prompts surface potential problems; users can accept, refine, or reject each. | • AI suggests ≥3 problems per idea.<br>• User can edit text and add tags.<br>• Accepted problems are stored with confidence score. |
| **FR‑4** | **Willingness‑to‑Pay Survey Generation** | System auto‑generates a short survey (≤5 questions) based on the validated problem. | • Survey includes price‑point, urgency, and demographic questions.<br>• Survey can be customized by the entrepreneur. |
| **FR‑5** | **Data Collection & Storage** | Survey responses are stored securely, linked to the originating idea and problem. | • Each response has a timestamp, respondent ID, and anonymized IP.<br>• GDPR‑compliant data retention policy. |
| **FR‑6** | **Analytics Dashboard** | Visualize validation metrics: problem confidence, WTP distribution, response rate, and trend graphs. | • Dashboard loads within 2 s.<br>• Charts are interactive (zoom, filter). |
| **FR‑7** | **Export & Reporting** | Users can export validated problems and WTP data as CSV or PDF. | • Export includes metadata, timestamps, and confidence scores.<br>• PDF follows Axentx branding. |
| **FR‑8** | **Pipeline Integration** | Validated problems are pushed to the Axentx shared BRAIN (pgvector) and the product backlog. | • API call to `/api/validate` triggers a message to the queue.<br>• Backlog entry includes problem text, tags, and WTP summary. |
| **FR‑9** | **Notification System** | Email/SMS alerts for new survey responses, low response rates, and validation milestones. | • Users receive email within 5 min of event.<br>• Opt‑in/opt‑out controls available. |
| **FR‑10** | **RESTful API** | Expose endpoints for external tools (e.g., CRM, analytics). | • Endpoints: `/ideas`, `/problems`, `/surveys`, `/responses`. <br>• All endpoints require OAuth 2.0 bearer token. |

---

## 3. Non‑Functional Requirements

| Category | Requirement | Details |
|----------|-------------|---------|
| **Performance** | Response Time | API responses ≤ 2 s under 1,000 concurrent users. |
| **Scalability** | Load Handling | Horizontal scaling to 10,000 concurrent users; auto‑scaling on Kubernetes. |
| **Reliability** | Uptime | 99.9 % uptime SLA (≤ 8.76 h downtime per year). |
| **Security** | Data Protection | • OAuth 2.0 + PKCE.<br>• TLS 1.3 for all traffic.<br>• AES‑256 encryption at rest (PostgreSQL). |
| **Privacy** | GDPR / CCPA | • Data minimization
