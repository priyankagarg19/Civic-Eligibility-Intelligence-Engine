# Design Document: Civic Eligibility Intelligence Engine (CEIE)

## 1. System Overview

### 1.1 Purpose

CEIE is an AI-powered platform that automatically matches citizens to government welfare schemes by extracting eligibility criteria from unstructured documents using NLP and calculating personalized eligibility scores.

**Core Problem**: Citizens cannot discover relevant schemes due to scattered information, complex criteria, and manual comparison burden. CEIE automates this entire workflow.

### 1.2 Why AI is Essential

AI is not optional—it's fundamental because:
- **NLP Required**: Eligibility rules exist in natural language documents requiring automated extraction
- **Dynamic Interpretation**: Criteria vary significantly across schemes; rule-based systems cannot scale
- **Continuous Learning**: System improves accuracy through admin corrections and retraining
- **Explainability**: AI provides transparent, criterion-level explanations for trust

### 1.3 MVP Scope (Hackathon)

**What's Included**:
- 5-10 pre-loaded schemes (single state)
- Citizen profile input → eligibility scoring → ranked results
- NLP criteria extraction (spaCy + rule-based patterns)
- Document checklist generation
- Explainable results with confidence scores
- Basic admin dashboard
- Local Docker deployment
- Response time: <10 seconds

**What's Excluded** (Future):
- Real-time scheme upload, multi-language, mobile app, Aadhaar integration, cloud deployment, advanced ML training


## 2. High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         PRESENTATION LAYER                          │
├─────────────────────────────────────────────────────────────────────┤
│  ┌──────────────────┐    ┌──────────────────┐   ┌──────────────┐  │
│  │   Citizen Web    │    │  Admin Dashboard │   │   REST API   │  │
│  │   Interface      │    │   (Scheme Mgmt)  │   │   Gateway    │  │
│  └──────────────────┘    └──────────────────┘   └──────────────┘  │
└───────────────────────────────────┬─────────────────────────────────┘
                                    │
┌───────────────────────────────────┼─────────────────────────────────┐
│                        APPLICATION LAYER                            │
├─────────────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐  ┌──────────────────┐  ┌──────────────────┐  │
│  │ Profile Manager │  │ Eligibility      │  │ Document         │  │
│  │                 │  │ Orchestrator     │  │ Generator        │  │
│  └─────────────────┘  └──────────────────┘  └──────────────────┘  │
└───────────────────────────────────┬─────────────────────────────────┘
                                    │
┌───────────────────────────────────┼─────────────────────────────────┐
│                           AI/NLP LAYER                              │
├─────────────────────────────────────────────────────────────────────┤
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐ │
│  │  NLP Extraction  │  │  Matching Engine │  │  Scoring Engine  │ │
│  │  (spaCy)         │  │  (Rule-based)    │  │  (Weighted)      │ │
│  └──────────────────┘  └──────────────────┘  └──────────────────┘ │
└───────────────────────────────────┬─────────────────────────────────┘
                                    │
┌───────────────────────────────────┼─────────────────────────────────┐
│                            DATA LAYER                               │
├─────────────────────────────────────────────────────────────────────┤
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐ │
│  │  PostgreSQL DB   │  │  Redis Cache     │  │  File Storage    │ │
│  │  (Profiles,      │  │  (Criteria,      │  │  (Scheme Docs)   │ │
│  │   Schemes)       │  │   Scores)        │  │                  │ │
│  └──────────────────┘  └──────────────────┘  └──────────────────┘ │
└─────────────────────────────────────────────────────────────────────┘
```

**Flow**: Citizen submits profile → Orchestrator retrieves schemes → Matching Engine evaluates criteria → Scoring Engine calculates scores → Ranking Engine sorts results → Document Generator creates checklist → Explainability adds details → Response returned


## 3. Component Design

### 3.1 Citizen Profile Module
- Captures demographic (age, gender, family), economic (income, occupation), geographic (state, district), social (caste, disability), educational data
- Validates inputs, stores with AES-256 encryption
- Profile versioning for audit trail

### 3.2 NLP Extraction Engine
**Purpose**: Extract structured eligibility rules from unstructured scheme documents

**Techniques**:
- **NER**: Identify age, income, location, occupation entities (spaCy + custom training)
- **Pattern Matching**: Regex for "must be", "should have", "eligible if"
- **Dependency Parsing**: Extract subject-verb-object relationships
- **Classification**: Categorize criteria (age_range, income_threshold, location_match, etc.)
- **Confidence Scoring**: 90-100 (auto-approve), 70-89 (audit), <70 (manual review)

**Output**: Structured criteria with type, operator, value, confidence score

### 3.3 Matching Engine
Compares citizen profile against each scheme's criteria using rule-based logic:
- Age: `profile.age BETWEEN criterion.value_min AND criterion.value_max`
- Income: `profile.annual_income < criterion.value_max`
- Location: `profile.state IN criterion.value_list`
- Occupation: `profile.occupation IN criterion.value_list`

Returns boolean match result for each criterion.

### 3.4 Scoring Engine
Calculates eligibility score (0-100):

```pseudocode
IF all mandatory criteria met:
  score = (matched_weight / total_weight) * 100
  IF perfect match: score += 5 bonus
ELSE:
  score = (mandatory_matched / mandatory_total) * 60  // capped at 60
```

**Score Ranges**:
- 95-100: Perfect match
- 80-94: High eligibility
- 60-79: Moderate eligibility
- 40-59: Partial (some mandatory missing)
- 0-39: Low eligibility

### 3.5 Ranking Engine
Sorts schemes by composite score:
- Eligibility score (70% weight)
- Benefit amount (20% weight)
- Application simplicity (10% weight - fewer documents = higher)

Groups by category: Healthcare, Education, Housing, Employment, Agriculture, Social Welfare

### 3.6 Document Checklist Generator
- Extracts required documents from scheme text using NLP
- Consolidates across multiple schemes (deduplicates)
- Indicates which documents citizen already has
- Prioritizes mandatory documents

### 3.7 Explainability Component
Provides transparent explanations:
- Criterion-level: "Your age (45) is within 18-60" ✓ or "Income (₹2.5L) exceeds ₹2L threshold" ✗
- Score breakdown: Mandatory 2/3 met, Optional 3/4 met
- Source citations: Links to original document text
- What-if analysis: "To qualify, reduce income to <₹2L"
- Confidence disclosure: Shows extraction confidence per criterion


## 4. AI Pipeline

```
┌──────────────────────────────────────────────────────────────────────┐
│                        AI PIPELINE WORKFLOW                          │
└──────────────────────────────────────────────────────────────────────┘

Stage 1: Document Ingestion
  Scheme PDF/DOCX → Text Extraction (PyPDF2, python-docx) → Clean Text

Stage 2: NLP Extraction
  Sentence Segmentation → NER (spaCy) → Pattern Matching → 
  Dependency Parsing → Criterion Classification → Confidence Scoring

Stage 3: Validation & Storage
  Admin Review (if confidence <70%) → Store in Database → Cache Criteria

Stage 4: Matching & Scoring
  Citizen Profile → Criteria Matching → Eligibility Scoring → 
  Scheme Ranking → Explainability Generation → Results to Citizen
```

**NLP Confidence Levels**:
- 90-100: Pattern match (auto-approve)
- 70-89: NER-based (audit flag)
- <70: Ambiguous (manual review required)

**Example Extraction**:
Input: "Applicant must be between 18 and 60 years of age and have an annual income below ₹2,00,000."

Output:
```
Criterion 1: {type: age_range, operator: between, value_min: 18, value_max: 60, confidence: 95}
Criterion 2: {type: income_threshold, operator: less_than, value_max: 200000, confidence: 92}
```


## 5. Data Flow

**Flow 1: Scheme Processing**
Admin uploads doc → Text extraction → NLP extraction → Criteria stored → Cache updated

**Flow 2: Eligibility Analysis**
Citizen submits profile → Orchestrator retrieves schemes → Matching engine evaluates → Scoring engine calculates → Ranking engine sorts → Document generator creates checklist → Explainability adds details → Results returned + audit logged

**Flow 3: Admin Review**
Admin reviews flagged criteria → Validates/corrects → Updates stored → Cache invalidated → Training data collected


## 6. Database Schema

### Core Tables

**Citizen_Profile**
```
profile_id (UUID, PK), age, gender, marital_status, family_size,
annual_income, employment_status, occupation, land_holding_acres,
state, district, city, postal_code, area_type,
caste_category, disability_status, minority_status, education_level,
government_id_hash (encrypted)
```

**Scheme_Document**
```
document_id (UUID, PK), scheme_name, issuing_department,
effective_date, expiration_date, benefit_amount, category,
document_path, text_content, language, status
```

**Eligibility_Criteria**
```
criteria_id (UUID, PK), document_id (FK),
criterion_type, operator, value_min, value_max, value_list (JSON),
source_text, source_location, confidence_score,
is_mandatory, weight, status
```

**Eligibility_Result**
```
result_id (UUID, PK), profile_id (FK), scheme_id (FK),
eligibility_score, ranking_score, analyzed_at, match_details (JSON)
```

**Document_Requirement**
```
requirement_id (UUID, PK), scheme_id (FK),
document_name, document_type, is_mandatory, description,
acceptable_formats (JSON), validity_period, issuing_authority
```

**Indexes**: profile_id, scheme_id, status, confidence_score, analyzed_at


## 7. Technology Stack (MVP)

**Frontend**: React + Material-UI  
**Backend**: FastAPI (Python)  
**NLP**: spaCy (en_core_web_sm) + custom patterns  
**Database**: PostgreSQL 14  
**Cache**: Redis 7  
**Deployment**: Docker + Docker Compose  
**Web Server**: Nginx (reverse proxy)  

**Libraries**:
- Text extraction: PyPDF2, python-docx, pdfplumber
- Validation: Pydantic
- Auth: JWT (python-jose)
- Encryption: cryptography (Fernet)


## 8. Deployment Architecture (Hackathon)

```
┌─────────────────────────────────────────────────────────────────┐
│                      Docker Host Machine                        │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                    Nginx Container                        │ │
│  │                   (Port 80/443)                           │ │
│  │  - Reverse Proxy                                          │ │
│  │  - Static File Serving (React build)                     │ │
│  └─────────────┬─────────────────────────────────────────────┘ │
│                │                                               │
│  ┌─────────────┴─────────────┐    ┌────────────────────────┐  │
│  │   Frontend Container      │    │   Backend Container    │  │
│  │   (React App)             │    │   (FastAPI)            │  │
│  │   Port: 3000              │    │   Port: 8000           │  │
│  └───────────────────────────┘    │  - REST API            │  │
│                                    │  - NLP Processing      │  │
│                                    └──────────┬─────────────┘  │
│                                               │                │
│                    ┌──────────────────────────┴────────┐       │
│                    │                                   │       │
│         ┌──────────▼──────────┐         ┌────────────▼──────┐ │
│         │  PostgreSQL         │         │  Redis            │ │
│         │  Container          │         │  Container        │ │
│         │  Port: 5432         │         │  Port: 6379       │ │
│         │  - Citizen Profiles │         │  - Criteria Cache │ │
│         │  - Schemes          │         │  - Session Data   │ │
│         │  - Criteria         │         └───────────────────┘ │
│         └─────────────────────┘                               │
│                                                               │
└─────────────────────────────────────────────────────────────────┘

Network: ceie_network (Docker bridge network)
```

**Deployment Steps**:
1. Install Docker & Docker Compose
2. Clone repository
3. Configure .env file (DB credentials, JWT secret)
4. Run `docker-compose build && docker-compose up -d`
5. Initialize database: `docker-compose exec backend python -m alembic upgrade head`
6. Seed data: `docker-compose exec backend python scripts/seed_data.py`
7. Access: http://localhost:3000 (citizen), http://localhost:3000/admin (admin)

**Resource Requirements**:
- Minimum: 4 CPU cores, 8GB RAM, 20GB storage
- Recommended: 8 CPU cores, 16GB RAM, 50GB storage


## 9. Security & Privacy

### Data Encryption
- **At Rest**: AES-256 for sensitive fields (government_id_hash, annual_income)
- **In Transit**: TLS 1.3 for all HTTP communications
- **Key Management**: Environment variables (MVP), AWS KMS (production)

### Authentication & Authorization
**Citizen Access**: No auth required for eligibility analysis (anonymous sessions)

**Admin Access**: JWT-based authentication with role-based access control (RBAC)
- Super_Admin: Full access
- Scheme_Manager: Upload/manage schemes
- Criteria_Reviewer: Review/validate extractions
- Auditor: Read-only logs and analytics

### Data Privacy
- Anonymization for analytics (hash profile_id, group age/income into brackets)
- Data retention: Profiles (1 year), Results (2 years), Audit logs (5 years)
- Right to deletion: Remove PII within 30 days, retain anonymized audit trail

### API Security
- Rate limiting: 10 requests/minute per IP
- Input validation: Pydantic models with constraints
- SQL injection prevention: Parameterized queries (SQLAlchemy ORM)
- XSS prevention: React auto-escaping + CSP headers


## 10. Scalability Strategy (Future)

### Current MVP Limitations
- Single Docker host (no distributed deployment)
- 5-10 schemes (limited coverage)
- Expected load: 100-500 profiles during demo
- No horizontal scaling

### Scaling Approach

**Phase 1: Stateless Backend Scaling**
- Multiple FastAPI instances behind load balancer
- Shared PostgreSQL + Redis
- Auto-scaling based on CPU/memory

**Phase 2: Database Scaling**
- Primary-replica setup (writes to primary, reads from replicas)
- Read/write splitting in application layer
- Replication lag: <1 second

**Phase 3: Caching Strategy**
- Level 1: Application cache (in-memory LRU, 100 items)
- Level 2: Redis cache (distributed, TTL: 1 hour for criteria)
- Level 3: Database (source of truth)

**Phase 4: Async Processing**
- Celery + Redis for background jobs
- NLP extraction as async task (2 min per document)
- Bulk eligibility analysis in batches

**Capacity Planning** (Production):
- Target: 1M citizens/year, 10% monthly active (100K)
- Peak load: 10x average = 1.2 req/sec
- Infrastructure: 4 backend instances (2 vCPU, 4GB each), 1 primary + 2 replica DBs (4 vCPU, 8GB each)


## 11. Risks & Mitigation

### Risk 1: Inaccurate NLP Extraction
**Impact**: High - Wrong eligibility determinations  
**Likelihood**: Medium  
**Mitigation**:
- Confidence thresholds (flag <70% for review)
- Human-in-the-loop validation
- Multiple extraction methods (patterns + ML)
- Continuous improvement via admin corrections

### Risk 2: Performance Degradation
**Impact**: Medium - Slow response times  
**Likelihood**: High - MVP not optimized  
**Mitigation**:
- Caching (Redis for criteria)
- Database indexing
- Async processing for NLP
- Load testing before launch

### Risk 3: Incomplete/Ambiguous Documents
**Impact**: High - Cannot extract criteria  
**Likelihood**: High - Document quality varies  
**Mitigation**:
- Document quality checks
- Admin review for ambiguous text
- Standardized templates (work with government)
- Confidence scoring to flag issues

### Risk 4: Outdated Scheme Information
**Impact**: High - Incorrect eligibility info  
**Likelihood**: Medium - Schemes updated periodically  
**Mitigation**:
- Expiration dates for schemes
- Automated alerts for expiring schemes
- Version control for scheme changes
- Disclaimer with last-updated date

### Risk 5: Data Breach
**Impact**: Critical - Privacy violation  
**Likelihood**: Low - With proper security  
**Mitigation**:
- AES-256 encryption + TLS 1.3
- RBAC for all operations
- Audit logging
- Regular security testing
- Data minimization

### Risk 6: Low User Adoption
**Impact**: Medium - Failure to achieve impact  
**Likelihood**: Medium - Trust in AI is low  
**Mitigation**:
- Explainability (criterion-level details)
- Transparency (confidence scores, source citations)
- User education (tutorials, help docs)
- Assisted access (partner with CSCs, NGOs)
- Success stories


## 12. Performance Targets

**Response Time Goals**:
- Profile creation: <1s
- Eligibility analysis (all schemes): <5s
- Single scheme analysis: <1s
- Document checklist generation: <2s
- Admin dashboard load: <2s

**Throughput Goals**:
- Concurrent users: 50 (MVP), 1000 (production)
- Eligibility analyses: 10/sec (MVP), 100/sec (production)
- NLP extraction: 1 document/2 min (background job)

**Accuracy Goals**:
- NLP extraction: 85% precision, 80% recall
- Eligibility determination: 85% agreement with manual review
- Admin correction rate: <15%


## 13. Post-Hackathon Roadmap

**Phase 1 (Months 1-3): Production Readiness**
- Multi-language support (Hindi + 2 regional languages)
- Cloud deployment (AWS/Azure)
- OAuth integration
- Comprehensive testing
- Security hardening

**Phase 2 (Months 4-6): Scale to State**
- Expand to 50+ schemes in one state
- Real-time scheme document upload
- Mobile-responsive design
- SMS notifications
- Integration with state portals

**Phase 3 (Months 7-12): National Expansion**
- Multi-state deployment (5+ states)
- Advanced NLP with custom training
- Mobile app (iOS/Android)
- Aadhaar integration
- DigiLocker integration
- Analytics dashboard

**Phase 4 (Year 2+): Advanced Features**
- AI-powered recommendations based on life events
- Predictive analytics for scheme utilization
- Chatbot interface
- Blockchain for audit trail
- API marketplace


## 14. Success Metrics

### Hackathon Demo Success Criteria
1. Functional eligibility analysis for 5+ schemes with >80% accuracy
2. End-to-end workflow completes in <10 seconds
3. Clear, explainable results with criterion-level feedback
4. Functional document checklist generation
5. Intuitive UI accessible to non-technical users
6. Live demo runs without crashes

### Production Success Metrics (Future)
- User adoption: 100K profiles in 6 months
- Accuracy: 85% agreement with manual reviews
- Efficiency: 50% reduction in time to identify schemes
- Impact: 30% increase in scheme application rates
- Performance: 99.5% uptime, <5s response time
- Security: Zero critical incidents


---
