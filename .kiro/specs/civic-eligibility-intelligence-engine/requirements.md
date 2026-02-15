# Requirements Document: Civic Eligibility Intelligence Engine (CEIE)

## Introduction

**One-Line Pitch:** An AI-powered eligibility intelligence engine that automatically matches citizens to the most relevant government welfare schemes and detects missed benefits.

The Civic Eligibility Intelligence Engine (CEIE) is an AI-powered decision-support platform designed to bridge the gap between citizens and government welfare schemes. The system analyzes structured citizen profile data and matches it against eligibility criteria extracted from official scheme documents using Natural Language Processing (NLP). By automating the complex process of eligibility determination, CEIE aims to increase scheme awareness, reduce administrative burden, and ensure that eligible citizens can access the benefits they deserve.

## Core Features

The CEIE platform consists of five integrated modules:

1. **Profile Intake Module**: Structured data collection capturing citizen demographic, economic, and social attributes
2. **Scheme Rule Extraction Engine**: NLP-based component that extracts eligibility criteria from unstructured scheme documents
3. **Eligibility Scoring Engine**: AI-powered matching system that compares profiles against scheme criteria and calculates eligibility scores
4. **Priority Ranking System**: Intelligent ranking algorithm that orders schemes by relevance and eligibility likelihood
5. **Document Requirement Generator**: Automated checklist creation for required application documents

These modules work together to transform the citizen experience from manual scheme discovery to automated, personalized recommendations.

## Problem Overview

### Problem Statement

Many government welfare schemes exist to support citizens, but a critical gap prevents eligible beneficiaries from accessing these benefits:

**Citizen Challenges:**
- Lack of awareness about available government schemes
- Scheme eligibility criteria are complex, scattered across multiple sources, and written in technical/legal language
- Rural citizens and low-income populations cannot manually compare dozens of schemes
- Citizens rely on middlemen or informal channels for information
- Eligible people miss benefits they deserve, leading to continued economic hardship

**Systemic Issues:**
- Underutilization of public welfare funds
- Welfare leakage - benefits not reaching intended beneficiaries
- Information asymmetry between government and citizens
- High administrative burden on government outreach workers

### Root Cause Analysis

1. **Unstructured Information**: Scheme rules exist in unstructured PDF documents without standardized formats
2. **Multi-Parameter Complexity**: Eligibility depends on multiple intersecting parameters (age, income, location, occupation, family status, etc.)
3. **No Centralized System**: No unified platform exists for eligibility matching across schemes
4. **Manual Processes**: Citizens must manually read and interpret each scheme document
5. **Scalability Limitations**: Human-driven outreach cannot scale to reach millions of eligible citizens

### Target Users

**Primary Users:**
- Rural households seeking welfare benefits
- Low-income citizens in urban and rural areas
- Small farmers and agricultural workers
- Informal sector workers without employer benefits
- Marginalized communities with limited digital literacy

**Secondary Users:**
- NGOs conducting welfare outreach programs
- Government outreach workers and field officers
- Community service centers (CSCs) and Jan Seva Kendras
- Social workers assisting vulnerable populations

### Solution Approach

CEIE addresses these challenges through an AI-powered workflow:

1. **Profile Intake**: Citizen enters structured profile data (demographic, economic, social attributes)
2. **AI-Powered Extraction**: NLP engine extracts eligibility rules from unstructured scheme documents
3. **Intelligent Matching**: Matching engine compares citizen profile against all scheme criteria
4. **Scoring & Ranking**: System calculates eligibility scores and ranks schemes by relevance
5. **Actionable Guidance**: Generates personalized document checklists for application preparation

### Why AI is Essential

AI is not optional for this problem - it is fundamental because:

- **Natural Language Understanding**: Scheme rules are written in natural language requiring NLP for extraction
- **Scalability**: Manual rule extraction cannot scale across hundreds of schemes and frequent updates
- **Dynamic Interpretation**: AI can interpret complex, multi-condition eligibility criteria dynamically
- **Variation Handling**: Eligibility conditions vary significantly across schemes, requiring adaptive matching
- **Continuous Learning**: System improves accuracy over time through feedback and retraining

### Expected Impact

**For Citizens:**
- Reduce information asymmetry and increase scheme awareness
- Increase scheme enrollment rates among eligible populations
- Eliminate dependency on middlemen
- Reduce time to identify relevant schemes from days to minutes

**For Government:**
- Improve policy utilization efficiency and fund deployment
- Reduce administrative burden on outreach workers
- Enable data-driven policy decisions based on eligibility patterns
- Scale welfare delivery across districts and states

**Measurable Outcomes:**
- 30% increase in scheme application rates among eligible citizens
- 50% reduction in time for citizens to identify eligible schemes
- 40% reduction in administrative workload for eligibility queries
- Potential to reach millions of underserved citizens nationwide

## Stakeholders

**Primary Stakeholders:**
- **Rural Households**: Families in rural areas seeking welfare benefits
- **Low-Income Citizens**: Urban and rural populations below poverty line
- **Small Farmers**: Agricultural workers and landholders seeking farming subsidies
- **Informal Workers**: Daily wage workers, street vendors, gig workers without formal employment
- **NGOs**: Non-profit organizations conducting welfare outreach
- **Government Outreach Workers**: Field officers, Anganwadi workers, ASHA workers

**Secondary Stakeholders:**
- **Government Administrators**: Officials managing scheme enrollment and verification
- **Policy Makers**: Government personnel defining and updating scheme criteria
- **System Administrators**: Technical staff maintaining the CEIE platform
- **Data Entry Operators**: Personnel responsible for inputting and updating scheme documents
- **Auditors**: Compliance officers ensuring accurate eligibility determinations
- **Community Service Centers**: CSCs and Jan Seva Kendras facilitating citizen access

## Glossary

- **CEIE**: Civic Eligibility Intelligence Engine - the complete system
- **Citizen_Profile**: Structured data containing demographic, economic, and social information about a citizen
- **Scheme_Document**: Official government document describing a welfare scheme and its eligibility criteria
- **Eligibility_Criteria**: Structured rules extracted from scheme documents that determine qualification
- **NLP_Engine**: Natural Language Processing component that extracts criteria from documents
- **Scoring_Engine**: Component that calculates eligibility scores based on profile matching
- **Eligibility_Score**: Numerical value (0-100) indicating likelihood of eligibility for a scheme
- **Scheme_Ranking**: Ordered list of schemes sorted by eligibility score
- **Document_Checklist**: List of required documents a citizen must provide for scheme application
- **Profile_Attribute**: Individual data field in a citizen profile (e.g., age, income, location)
- **Matching_Engine**: Component that compares citizen profiles against eligibility criteria
- **Scheme_Repository**: Database storing all scheme documents and extracted criteria
- **Audit_Log**: Record of all eligibility determinations and system decisions

## Requirements

### Requirement 1: Citizen Profile Management

**User Story:** As a citizen, I want to input and manage my profile information, so that the system can accurately assess my eligibility for government schemes.

#### Acceptance Criteria

1. THE CEIE SHALL accept structured citizen profile data including demographic, economic, and social attributes
2. WHEN a citizen submits profile data, THE CEIE SHALL validate all required fields are present and properly formatted
3. WHEN profile data contains invalid values, THE CEIE SHALL return specific validation errors for each invalid field
4. THE CEIE SHALL store citizen profiles securely with encryption at rest
5. WHEN a citizen updates their profile, THE CEIE SHALL maintain a history of profile changes for audit purposes
6. THE CEIE SHALL support profile attributes including age, gender, income, location, occupation, family size, disability status, and education level

### Requirement 2: Scheme Document Ingestion

**User Story:** As a government administrator, I want to upload scheme documents to the system, so that eligibility criteria can be extracted and used for matching.

#### Acceptance Criteria

1. WHEN an administrator uploads a scheme document, THE CEIE SHALL accept documents in PDF, DOCX, and TXT formats
2. WHEN a document is uploaded, THE CEIE SHALL extract text content and store it in the Scheme_Repository
3. IF a document upload fails, THEN THE CEIE SHALL return a descriptive error message indicating the failure reason
4. THE CEIE SHALL associate each scheme document with metadata including scheme name, issuing department, effective date, and expiration date
5. WHEN a scheme document is updated, THE CEIE SHALL maintain previous versions for audit purposes
6. WHERE batch upload is supported, THE CEIE SHALL allow upload of multiple scheme documents simultaneously

### Requirement 3: Eligibility Criteria Extraction

**User Story:** As a system administrator, I want the NLP engine to automatically extract eligibility criteria from scheme documents, so that manual rule creation is minimized.

#### Acceptance Criteria

1. WHEN a scheme document is ingested, THE NLP_Engine SHALL extract eligibility criteria as structured rules
2. THE NLP_Engine SHALL identify criteria types including age ranges, income thresholds, geographic restrictions, occupation requirements, and demographic conditions
3. WHEN criteria extraction is complete, THE NLP_Engine SHALL store extracted rules in a structured format with confidence scores
4. IF the NLP_Engine cannot extract criteria with confidence above 70%, THEN THE CEIE SHALL flag the document for manual review
5. THE CEIE SHALL allow administrators to review and correct extracted criteria before activation
6. WHEN criteria are extracted, THE NLP_Engine SHALL link each criterion to its source text location in the original document

### Requirement 4: Eligibility Matching and Scoring

**User Story:** As a citizen, I want the system to calculate my eligibility for schemes, so that I can identify which programs I qualify for.

#### Acceptance Criteria

1. WHEN a citizen requests eligibility analysis, THE Matching_Engine SHALL compare the Citizen_Profile against all active Eligibility_Criteria
2. THE Scoring_Engine SHALL calculate an Eligibility_Score between 0 and 100 for each scheme based on profile match quality
3. WHEN all required criteria are met, THE Scoring_Engine SHALL assign a score of 90 or above
4. WHEN partial criteria are met, THE Scoring_Engine SHALL assign a score proportional to the percentage of criteria satisfied
5. WHEN no criteria are met, THE Scoring_Engine SHALL assign a score of 0
6. THE CEIE SHALL complete eligibility analysis for a single profile against all schemes within 5 seconds

### Requirement 5: Scheme Ranking and Recommendations

**User Story:** As a citizen, I want to see schemes ranked by my eligibility, so that I can prioritize which programs to apply for.

#### Acceptance Criteria

1. WHEN eligibility analysis is complete, THE CEIE SHALL generate a Scheme_Ranking ordered by Eligibility_Score in descending order
2. THE CEIE SHALL include only schemes with Eligibility_Score above 50 in the ranking
3. WHEN displaying ranked schemes, THE CEIE SHALL show scheme name, eligibility score, brief description, and benefit amount
4. THE CEIE SHALL group schemes by category (healthcare, education, housing, employment, social welfare)
5. WHERE a citizen profile matches multiple schemes in the same category, THE CEIE SHALL prioritize schemes with higher benefit amounts when scores are equal

### Requirement 6: Document Checklist Generation

**User Story:** As a citizen, I want to receive a checklist of required documents for each scheme, so that I can prepare my application efficiently.

#### Acceptance Criteria

1. WHEN a citizen selects a scheme, THE CEIE SHALL generate a Document_Checklist containing all required documents for that scheme
2. THE CEIE SHALL extract document requirements from scheme documents using the NLP_Engine
3. THE Document_Checklist SHALL include document name, description, and whether it is mandatory or optional
4. WHEN multiple schemes are selected, THE CEIE SHALL generate a consolidated checklist eliminating duplicate documents
5. THE CEIE SHALL indicate which documents from the checklist the citizen has already uploaded to their profile

### Requirement 7: Explainability and Transparency

**User Story:** As a citizen, I want to understand why I am eligible or ineligible for a scheme, so that I can trust the system's recommendations.

#### Acceptance Criteria

1. WHEN displaying an eligibility score, THE CEIE SHALL provide an explanation listing which criteria were met and which were not met
2. THE CEIE SHALL cite specific sections of the scheme document that define each criterion
3. WHEN a citizen is ineligible, THE CEIE SHALL indicate which profile attributes need to change to achieve eligibility
4. THE CEIE SHALL display the confidence level of the NLP_Engine's criteria extraction for each rule
5. THE CEIE SHALL allow citizens to view the original scheme document text alongside extracted criteria

### Requirement 8: Audit and Compliance

**User Story:** As an auditor, I want to review all eligibility determinations made by the system, so that I can ensure accuracy and compliance.

#### Acceptance Criteria

1. THE CEIE SHALL maintain an Audit_Log recording all eligibility determinations with timestamp, citizen identifier, scheme identifier, and eligibility score
2. WHEN an eligibility determination is made, THE CEIE SHALL log the profile attributes used and the criteria evaluated
3. THE CEIE SHALL allow auditors to filter audit logs by date range, scheme, score range, and determination outcome
4. THE CEIE SHALL retain audit logs for compliance with applicable data retention policies
5. WHEN an administrator modifies extracted criteria, THE CEIE SHALL log the change with administrator identifier, timestamp, and reason
6. WHERE compliance reporting is required, THE CEIE SHALL generate reports showing accuracy metrics comparing system determinations to manual reviews

### Requirement 9: NLP Model Training and Improvement

**User Story:** As a system administrator, I want to improve the NLP engine's accuracy over time, so that criteria extraction becomes more reliable.

#### Acceptance Criteria

1. WHEN administrators correct extracted criteria, THE CEIE SHALL store corrections as training data for the NLP_Engine
2. WHERE model retraining is supported, THE CEIE SHALL enable periodic retraining of the NLP_Engine using accumulated correction data
3. THE CEIE SHALL track extraction accuracy metrics including precision, recall, and F1 score for each criteria type
4. WHEN NLP_Engine accuracy falls below 80% for any criteria type, THE CEIE SHALL alert administrators
5. WHERE A/B testing is supported, THE CEIE SHALL enable testing of different NLP models before production deployment

### Requirement 10: Multi-Language Support

**User Story:** As a citizen, I want to use the system in my preferred language, so that I can understand eligibility information clearly.

#### Acceptance Criteria

1. THE CEIE SHALL support scheme documents in multiple languages including English, Hindi, and regional languages
2. WHEN a citizen selects a language preference, THE CEIE SHALL display all user interface text in that language
3. THE NLP_Engine SHALL extract criteria from documents in the document's original language
4. THE CEIE SHALL translate eligibility explanations and document checklists to the citizen's preferred language
5. WHEN translation confidence is below 85%, THE CEIE SHALL display both original and translated text

### Requirement 11: Data Privacy and Security

**User Story:** As a citizen, I want my personal information protected, so that my privacy is maintained while using the system.

#### Acceptance Criteria

1. THE CEIE SHALL encrypt all Citizen_Profile data at rest using AES-256 encryption
2. THE CEIE SHALL encrypt all data in transit using TLS 1.3 or higher
3. WHEN a citizen requests data deletion, THE CEIE SHALL remove all profile data within 30 days while retaining anonymized audit logs
4. THE CEIE SHALL implement role-based access control restricting profile access to authorized personnel only
5. THE CEIE SHALL mask sensitive profile attributes in logs and audit trails
6. WHEN a data breach is detected, THE CEIE SHALL alert administrators within 1 minute

### Requirement 12: System Integration

**User Story:** As a government administrator, I want CEIE to integrate with existing government systems, so that data can flow seamlessly between platforms.

#### Acceptance Criteria

1. THE CEIE SHALL provide a REST API for external systems to submit citizen profiles and retrieve eligibility results
2. WHERE API authentication is required, THE CEIE SHALL support OAuth 2.0 or API key-based authentication
3. WHEN an external system requests eligibility analysis, THE CEIE SHALL return results in JSON format
4. WHERE webhook support is enabled, THE CEIE SHALL provide webhook notifications to external systems when eligibility determinations are complete
5. THE CEIE SHALL provide API endpoints for scheme document upload and criteria management
6. THE CEIE SHALL implement rate limiting to prevent API abuse

### Requirement 13: Performance and Scalability

**User Story:** As a system administrator, I want the system to handle high volumes of requests, so that all citizens can access the service without delays.

#### Acceptance Criteria

1. THE CEIE SHALL be designed to support concurrent eligibility analysis for multiple citizen profiles
2. WHEN system load increases, THE CEIE SHALL be architected to scale horizontally by adding compute resources
3. THE CEIE SHALL maintain response times under 5 seconds for eligibility analysis under normal load
4. THE CEIE SHALL process scheme document ingestion and criteria extraction as background jobs without blocking user requests
5. THE CEIE SHALL cache frequently accessed scheme criteria to reduce database queries

### Requirement 14: Monitoring and Alerting

**User Story:** As a system administrator, I want to monitor system health and performance, so that I can proactively address issues.

#### Acceptance Criteria

1. THE CEIE SHALL expose metrics including request rate, response time, error rate, and NLP extraction accuracy
2. WHEN error rate exceeds 5% over a 5-minute window, THE CEIE SHALL send alerts to administrators
3. THE CEIE SHALL log all errors with stack traces and contextual information for debugging
4. THE CEIE SHALL provide a health check endpoint returning system status and component availability
5. THE CEIE SHALL track and report on scheme coverage (percentage of schemes with extracted criteria)

## Non-Functional Requirements

### Performance

- Eligibility analysis SHALL complete within 5 seconds for a single profile against all schemes
- NLP criteria extraction SHALL process a 10-page document within 2 minutes
- The system SHALL be designed to scale horizontally to support district-level deployment
- API response time SHALL be under 500ms for 95% of requests under normal load

### Reliability

- System uptime SHALL be 99.5% or higher for production deployment
- Data backup SHALL occur every 24 hours with retention for 90 days
- The system SHALL recover from failures within 15 minutes

### Usability

- The user interface SHALL be accessible and comply with WCAG 2.1 Level AA standards
- Citizens SHALL be able to complete profile creation within 10 minutes
- The system SHALL provide contextual help and tooltips for all input fields

### Maintainability

- The system SHALL use modular architecture with clear separation between NLP, matching, and scoring components
- All code SHALL include inline documentation and API specifications
- The system SHALL support zero-downtime deployments for updates

## AI Components

### NLP Engine

- **Purpose**: Extract eligibility criteria from unstructured scheme documents
- **Techniques**: Named Entity Recognition (NER), Relation Extraction, Text Classification
- **Input**: Scheme documents in PDF, DOCX, or TXT format
- **Output**: Structured eligibility rules with confidence scores
- **Training Data**: Government scheme documents with manually annotated criteria
- **Accuracy Target**: 85% precision and 80% recall for criteria extraction

### Matching Engine

- **Purpose**: Compare citizen profiles against eligibility criteria
- **Techniques**: Rule-based matching with fuzzy logic for partial matches
- **Input**: Citizen_Profile and Eligibility_Criteria
- **Output**: Boolean match result for each criterion
- **Logic**: Exact matching for categorical attributes, range matching for numerical attributes

### Scoring Engine

- **Purpose**: Calculate eligibility scores based on matching results
- **Techniques**: Weighted scoring algorithm considering criterion importance
- **Input**: Matching results from Matching_Engine
- **Output**: Eligibility_Score (0-100)
- **Algorithm**: Weighted sum of matched criteria divided by total criteria weight

## Data Requirements

### Citizen Profile Data

- **Demographic**: Age, gender, marital status, family size
- **Economic**: Annual income, employment status, occupation, assets
- **Geographic**: State, district, city, postal code, rural/urban classification
- **Social**: Caste category, disability status, minority status
- **Educational**: Highest qualification, current enrollment status
- **Identity**: Government ID numbers (anonymized for privacy)

### Scheme Document Data

- **Metadata**: Scheme name, department, effective date, expiration date, benefit amount
- **Content**: Full text of eligibility criteria, application process, required documents
- **Structure**: Sections, subsections, bullet points, tables
- **Version**: Document version number, last updated date

### Extracted Criteria Data

- **Criterion Type**: Age, income, location, occupation, demographic, educational
- **Operator**: Greater than, less than, equal to, in range, in list
- **Value**: Threshold value or list of acceptable values
- **Confidence**: NLP extraction confidence score (0-100)
- **Source**: Reference to original document text location

## Assumptions

1. Government scheme documents are available in digital format
2. Citizen profile data is accurate and up-to-date
3. Scheme eligibility criteria are clearly stated in official documents
4. Citizens have basic digital literacy to use the platform
5. Government departments will provide timely updates when schemes change
6. Internet connectivity is available for system access
7. The system will initially support schemes from a single state or region before national expansion

## Constraints

1. The system must comply with national data protection and privacy regulations
2. NLP models must be trained on government-approved datasets only
3. The system cannot make final eligibility determinations - human verification is required for enrollment
4. Budget limitations may restrict the number of languages supported initially
5. Integration with legacy government systems may require custom adapters
6. The system must operate within existing government IT infrastructure and security policies
7. Scheme documents may contain ambiguous or contradictory criteria requiring manual resolution

## Success Metrics

### User Adoption

- 100,000 citizen profiles created within 6 months of launch
- 70% of users complete eligibility analysis within first session
- 60% user retention rate after 3 months

### Accuracy

- 85% agreement between CEIE eligibility determinations and manual reviews
- NLP criteria extraction accuracy of 85% precision and 80% recall
- Less than 5% of eligibility determinations require manual correction

### Efficiency

- 50% reduction in time for citizens to identify eligible schemes (compared to manual search)
- 40% reduction in administrative workload for processing eligibility queries
- Average eligibility analysis completion time under 3 seconds

### Impact

- 30% increase in scheme application rates among eligible citizens
- 25% reduction in ineligible applications submitted
- 90% user satisfaction score for clarity of eligibility explanations

### System Performance

- 99.5% system uptime
- 95% of API requests completed within 500ms
- Zero critical security incidents

## MVP Scope (Hackathon Prototype)

For the hackathon demonstration, the CEIE prototype will implement a focused subset of features to prove the core concept:

### MVP Features

**Scheme Coverage:**
- Support 5-10 government schemes from one state (e.g., Maharashtra or Karnataka)
- Pre-uploaded scheme documents in the system
- Focus on high-impact schemes (PM-KISAN, crop insurance, housing, education subsidies)

**NLP Implementation:**
- Use pre-trained NLP models (spaCy, Hugging Face transformers) for criteria extraction
- Rule-based extraction for structured criteria (age, income thresholds)
- Manual validation of extracted criteria for demo schemes

**Eligibility Matching:**
- Core profile attributes: age, income, location, occupation, family size
- Basic matching logic for numerical ranges and categorical values
- Eligibility scoring based on percentage of criteria met

**User Interface:**
- Simple web form for profile input
- Results page showing ranked schemes with eligibility scores
- Document checklist generation for top 3 schemes

**Language Support:**
- Primary language: English or Hindi (one language for MVP)
- UI text and scheme descriptions in selected language

**Data Storage:**
- Local database (SQLite or PostgreSQL) for profiles and schemes
- No cloud deployment required for hackathon demo

### MVP Exclusions (Future Enhancements)

The following features are documented in requirements but excluded from the hackathon MVP:
- Multi-language support (beyond one primary language)
- Real-time scheme document upload and processing
- Advanced NLP model training and A/B testing
- OAuth integration and external API access
- Horizontal scaling and load balancing
- Webhook notifications
- Comprehensive audit logging (basic logging only)

### Success Criteria for MVP

The hackathon prototype will be considered successful if it demonstrates:
1. Accurate eligibility scoring for 5+ schemes with 80%+ precision
2. End-to-end workflow from profile input to ranked results in under 10 seconds
3. Clear, explainable eligibility decisions with criterion-level feedback
4. Functional document checklist generation
5. Clean, intuitive user interface accessible to non-technical users

## Sample Use Case

### Citizen Profile

**Name:** Ramesh Kumar (anonymized)  
**Age:** 45 years  
**Annual Income:** ₹1.5 lakh/year  
**Occupation:** Small farmer  
**Land Holding:** 2 acres  
**Location:** Rural Maharashtra (Ahmednagar district)  
**Family Size:** 4 members (self, spouse, 2 children)  
**Education:** 10th standard  
**Category:** General

### System Processing

1. Citizen enters profile data through web form
2. CEIE Matching Engine compares profile against all scheme criteria
3. Scoring Engine calculates eligibility scores
4. System ranks schemes by relevance

### System Output

**Ranked Eligible Schemes:**

1. **PM-KISAN (Pradhan Mantri Kisan Samman Nidhi)** - 92% Eligible
   - Benefit: ₹6,000/year in 3 installments
   - Match Reason: Small farmer with <2 hectares land
   - Missing Criteria: None
   - Required Documents: Aadhaar, Land records, Bank account details

2. **Pradhan Mantri Fasal Bima Yojana (Crop Insurance)** - 85% Eligible
   - Benefit: Crop insurance coverage up to sum insured
   - Match Reason: Farmer in notified area, eligible crops
   - Missing Criteria: Must be cultivating notified crops
   - Required Documents: Aadhaar, Land records, Bank account, Crop details

3. **Pradhan Mantri Awas Yojana - Gramin (Rural Housing)** - 65% Eligible
   - Benefit: ₹1.2 lakh for house construction
   - Match Reason: Rural location, income below threshold
   - Missing Criteria: Must not own pucca house (requires verification)
   - Required Documents: Aadhaar, Income certificate, Land ownership proof, BPL card

4. **Kisan Credit Card** - 78% Eligible
   - Benefit: Credit facility for agricultural needs
   - Match Reason: Farmer with land ownership
   - Missing Criteria: None
   - Required Documents: Aadhaar, Land records, Bank account

**Consolidated Document Checklist:**
- ✓ Aadhaar Card (required for all schemes)
- ✓ Land Ownership Records (required for 3 schemes)
- ✓ Bank Account Details (required for all schemes)
- ○ Income Certificate (required for 1 scheme)
- ○ BPL Card (if applicable, for 1 scheme)
- ○ Crop Cultivation Details (required for 1 scheme)

### User Action

Ramesh can now:
1. Prioritize applying for PM-KISAN (highest eligibility, immediate benefit)
2. Prepare consolidated documents for multiple applications
3. Visit nearest CSC or Jan Seva Kendra with required documents
4. Track application status through scheme portals

### Impact Demonstration

**Without CEIE:** Ramesh would need to manually research dozens of schemes, read complex eligibility criteria, and might miss PM-KISAN entirely.

**With CEIE:** Ramesh discovers 4 relevant schemes in under 2 minutes, understands his eligibility clearly, and has a concrete action plan with document requirements.
