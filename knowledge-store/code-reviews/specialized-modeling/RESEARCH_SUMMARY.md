# Specialized Modeling Tools Research Summary

**Research Date:** November 8, 2025
**Purpose:** Identify industry-leading open source tools across 5 modeling domains for potential deep analysis

---

## 1. System Architecture Modelers

### 1.1 ArchiMate Tools

#### **Archi** ⭐ RECOMMENDED FOR DEEP ANALYSIS
- **GitHub:** https://github.com/archimatetool/archi
- **License:** MIT
- **Description:** Free, open source, cross-platform ArchiMate modeling tool. The most popular ArchiMate tool with ~6,000 downloads/month. Supports ArchiMate 3.2 language specification.
- **Activity:** Active development through November 2025
- **Key Features:**
  - Java/Eclipse RCP based (Windows, macOS, Linux)
  - Scripting support via jArchi plugin
  - Collaboration via coArchi plugin
  - Large user community
- **Worth Deep Analysis:** ✅ YES - Industry standard for ArchiMate modeling, active community, extensible architecture

### 1.2 C4 Model Tools

#### **Structurizr** ⭐ RECOMMENDED FOR DEEP ANALYSIS
- **GitHub:** https://github.com/structurizr (multiple repos)
- **License:** Mixed (varies by component)
- **Description:** Diagrams as code for software architecture. Create multiple architecture diagrams from a single model using DSL.
- **Activity:** Active development through November 2025
- **Key Features:**
  - Structurizr DSL (1.4k stars)
  - Multi-language support (Java 1.1k stars, .NET 410 stars)
  - CLI, Lite, and cloud service options
  - PlantUML export capability
- **Worth Deep Analysis:** ✅ YES - Pioneer in architecture-as-code, strong adoption

#### **C4-PlantUML** ⭐ RECOMMENDED FOR DEEP ANALYSIS
- **GitHub:** https://github.com/plantuml-stdlib/C4-PlantUML
- **License:** MIT
- **Description:** Combines PlantUML with C4 model for simple way to describe and communicate software architectures
- **Activity:** Active development through 2025 (v1.2025.1+)
- **Key Features:**
  - Text-based diagrams
  - GitHub preview support
  - Wide PlantUML ecosystem compatibility
- **Worth Deep Analysis:** ✅ YES - Highly accessible, no special tools needed, version control friendly

#### **Mermaid.js (C4 Diagrams)**
- **GitHub:** https://github.com/mermaid-js/mermaid
- **License:** MIT
- **Description:** JavaScript-based diagramming with built-in C4 support. GitHub native rendering.
- **Worth Deep Analysis:** ⚠️ MAYBE - Good for documentation-as-code, but less specialized than dedicated tools

### 1.3 SysML System Modeling Tools

#### **Eclipse SysON** ⭐ RECOMMENDED FOR DEEP ANALYSIS
- **GitHub:** https://github.com/eclipse-syson (implied from Eclipse project)
- **Website:** https://mbse-syson.org/
- **License:** Eclipse Public License (presumed)
- **Description:** Open-source, web-based MBSE tool for SysML v2 models. Graphical, form-based, and tabular editors accessible from browsers.
- **Activity:** Active development in 2025, preparing for large-scale professional deployment
- **Key Features:**
  - SysML v2 compliant
  - Web-based (no desktop installation)
  - Industrial partnerships
- **Worth Deep Analysis:** ✅ YES - Next-gen SysML v2, modern architecture, active development

#### **Gaphor**
- **GitHub:** https://github.com/gaphor/gaphor
- **License:** Apache 2.0
- **Description:** UML, SysML, RAAML, and C4 modeling application. 100% open source, Python-based.
- **Activity:** Version 3.0 released in 2025
- **Key Features:**
  - Cross-platform (Windows, macOS, Linux)
  - Multiple modeling languages
  - Friendly Apache 2 license
- **Worth Deep Analysis:** ⚠️ MAYBE - Multi-language support is useful, but less specialized than SysON

#### **TTool**
- **Website:** https://ttool.telecom-paris.fr/
- **License:** Open source (specific license unclear)
- **Description:** Toolkit for UML and SysML diagram edition, simulation, and formal verification (safety, security, performance)
- **Worth Deep Analysis:** ⚠️ MAYBE - Interesting verification capabilities, but limited visibility/adoption

---

## 2. Data Lineage Modelers

### 2.1 Modern Metadata Platforms

#### **DataHub (LinkedIn)** ⭐ RECOMMENDED FOR DEEP ANALYSIS
- **GitHub:** https://github.com/datahub-project/datahub
- **License:** Apache 2.0
- **Description:** LinkedIn's modern data catalog platform with advanced lineage capabilities including column-level lineage tracking.
- **Activity:** v0.11.0 released September 2023, continued development through 2025
- **Key Features:**
  - Column-level lineage with Airflow DAG visualization
  - Modern UI (refreshed in 2023)
  - Deep integration with data ecosystem
  - Strong industry adoption
- **Worth Deep Analysis:** ✅ YES - Industry leader, comprehensive features, active development

#### **OpenMetadata** ⭐ RECOMMENDED FOR DEEP ANALYSIS
- **GitHub:** https://github.com/open-metadata/OpenMetadata
- **License:** Apache 2.0
- **Description:** Unified metadata platform for data discovery, observability, and governance. Built by founders of Apache Hadoop, Apache Atlas, and Uber Databook.
- **Activity:** v1.8.8 released July 2025, active through November 2025
- **Key Stats:** ~7.2k stars, fastest-growing open source data catalog
- **Key Features:**
  - 84+ connectors (100+ in ingestion framework)
  - Column-level lineage
  - Data contracts and governance
  - Central metadata repository
- **Worth Deep Analysis:** ✅ YES - Rapid growth, comprehensive connectors, modern architecture

#### **Amundsen (Lyft)**
- **GitHub:** https://github.com/amundsen-io/amundsen
- **License:** Apache 2.0
- **Description:** Metadata-driven application for data analysts, scientists, and engineers. Focuses on data discovery with "Google-like" search.
- **Key Features:**
  - PageRank-inspired search algorithm
  - Popularity-based ranking
  - Data lineage visualization
  - Intuitive discovery experience
- **Worth Deep Analysis:** ⚠️ MAYBE - Excellent search UX, but less comprehensive than DataHub/OpenMetadata

### 2.2 Apache Ecosystem

#### **Apache Atlas**
- **GitHub:** https://github.com/apache/atlas
- **License:** Apache 2.0
- **Description:** One of the first open-source tools for search, discovery, and governance in Hadoop ecosystem.
- **Key Features:**
  - Sophisticated classification systems (PII, SENSITIVE, EXPIRES_ON)
  - Auto-propagation via lineage
  - Deep integration with Apache Ranger
  - Fine-grained security and data masking
- **Worth Deep Analysis:** ⚠️ MAYBE - Mature and powerful, but Hadoop-centric; newer tools offer broader platform support

### 2.3 Lineage Standards & Reference Implementations

#### **OpenLineage + Marquez** ⭐ RECOMMENDED FOR DEEP ANALYSIS
- **GitHub:**
  - https://github.com/OpenLineage/OpenLineage
  - https://github.com/MarquezProject/marquez
- **License:** Apache 2.0
- **Description:** OpenLineage is open standard for lineage metadata collection (LF AI & Data Foundation Graduate project). Marquez is the reference implementation.
- **Activity:** Active development through 2025 (Copyright 2018-2025)
- **Key Features:**
  - Platform-agnostic lineage standard
  - Integrations: Apache Airflow, Spark, Flink, dbt, Dagster
  - Visual UI for lineage mapping
  - Job-level and dataset-level tracking
- **Worth Deep Analysis:** ✅ YES - Emerging standard, vendor-neutral, strong integration ecosystem

---

## 3. FMEA (Failure Mode and Effects Analysis) Modelers

### 3.1 Open Source FMEA Tools

#### **Open FMEA**
- **GitHub:** https://github.com/ngmgithub/open-fmea
- **SourceForge:** https://sourceforge.net/projects/openfmea/
- **License:** Open source (specific license unclear)
- **Description:** Web-based tool for crowdsourced FMEA generation. Replaces subjective rankings with relative rankings via comparative analysis.
- **Activity:** Active development through February 2025
- **Key Features:**
  - Distributed team collaboration
  - Crowdsourcing approach
  - Relative comparison methodology
- **Worth Deep Analysis:** ⚠️ MAYBE - Innovative approach, but limited adoption/maturity

#### **FMEAApp (Multiple Forks)**
- **GitHub:**
  - https://github.com/Foundliew/openfmea
  - https://github.com/dromation/open-fmea
- **License:** Not specified
- **Description:** Desktop application with interactive FMEA tables
- **Key Features:**
  - Editable cells (Severity, Occurrence, Detection)
  - Automatic RPN calculation with color coding
  - Multiple export formats (text, JSON, XML, SQL)
- **Worth Deep Analysis:** ❌ NO - Limited scope, appears to be student/learning projects

#### **FMECAengine**
- **GitHub:** https://github.com/ovitrac/FMECAengine
- **License:** Not specified
- **Description:** FMECA software developed for SafeFoodPack Design project
- **Worth Deep Analysis:** ❌ NO - Specialized domain, limited general applicability

#### **NASA FMEA Tool**
- **Source:** NASA Software Catalog (MSC-25379-1)
- **License:** NASA Open Source
- **Description:** Prototype tool that models system components, relationships, and functions to semi-automatically generate FMEA models early in design lifecycle
- **Worth Deep Analysis:** ⚠️ MAYBE - Interesting semi-automated approach, but prototype status unclear

### 3.2 Commercial Dominance Note

The FMEA software market is dominated by commercial solutions (ReliaSoft XFMEA, Sphera FMEA-Pro, Siemens, etc.). Open source options are limited and generally less mature. Organizations seeking free FMEA tools may need to accept limited features or consider the NASA prototype.

**Recommendation:** This domain has weak open source offerings. **NOT recommended for deep analysis** unless specific interest in emerging crowdsourced approaches.

---

## 4. Impact Assessment Modelers

### 4.1 Dependency Analysis Tools

#### **OWASP Dependency-Track** ⭐ RECOMMENDED FOR DEEP ANALYSIS
- **GitHub:** https://github.com/DependencyTrack/dependency-track
- **License:** Apache 2.0
- **Description:** Open source platform with web UI for continuous component vulnerability tracking
- **Activity:** Active development through October 2025
- **Key Features:**
  - Continuous monitoring across portfolio
  - Live view of risk
  - Version tracking
  - Web-based dashboard
- **Worth Deep Analysis:** ✅ YES - Comprehensive platform, active OWASP project, enterprise-ready

#### **OWASP Dependency-Check**
- **GitHub:** https://github.com/jeremylong/DependencyCheck
- **License:** Apache 2.0
- **Description:** CLI and build plugin to scan project dependencies for known vulnerabilities
- **Key Features:**
  - Multi-language support
  - CLI and CI/CD integration
  - Actively maintained by OWASP
- **Worth Deep Analysis:** ⚠️ MAYBE - Essential security tool, but focused on vulnerabilities rather than general impact analysis

#### **Trivy**
- **GitHub:** https://github.com/aquasecurity/trivy
- **License:** Apache 2.0
- **Description:** Comprehensive security scanner for containers and other artifacts
- **Key Features:**
  - Container scanning
  - GitHub Actions integration
  - Infrastructure as Code scanning
- **Worth Deep Analysis:** ⚠️ MAYBE - Excellent security tool, but narrow focus on vulnerabilities

#### **JDepend**
- **GitHub:** https://github.com/clarkware/jdepend
- **License:** BSD
- **Description:** Lightweight tool for Java package dependency analysis and design quality metrics
- **Worth Deep Analysis:** ❌ NO - Java-specific, limited to package metrics

### 4.2 Change Impact Analysis

#### **JRipples**
- **SourceForge:** https://sourceforge.net/projects/jripples/
- **GitHub (Swing Port):** https://github.com/Javier12/jswingripples
- **License:** Open source (specific license unclear)
- **Description:** Eclipse plugin for Java change impact analysis. Supports impact analysis and change propagation.
- **Key Features:**
  - Intelligent assistance philosophy
  - Component tracking
  - Inconsistency detection
- **Worth Deep Analysis:** ❌ NO - Eclipse/Java specific, appears unmaintained, limited to IDE context

### 4.3 Blast Radius Calculators

#### **Blast Radius (Terraform)** ⭐ RECOMMENDED FOR DEEP ANALYSIS
- **GitHub:** https://github.com/28mm/blast-radius
- **Fork (Maintained):** https://github.com/Ianyliu/blast-radius-fork
- **License:** MIT
- **Description:** Interactive visualizations of Terraform dependency graphs using d3.js
- **Activity:** Original repo inactive since 2020; active fork with compatibility for recent Terraform/Python versions
- **Key Features:**
  - Graphviz layout
  - PyHCL for Terraform parsing
  - d3.js interactive visualization
  - Docker and pip installation
- **Worth Deep Analysis:** ✅ YES - Valuable for IaC impact analysis, visual approach, active fork

#### **SonarQube**
- **GitHub:** https://github.com/SonarSource/sonarqube
- **License:** LGPL-3.0
- **Description:** Code quality and security analysis platform with dependency management
- **Worth Deep Analysis:** ⚠️ MAYBE - Comprehensive platform, but broad focus beyond impact analysis

### 4.4 Domain Assessment

Impact analysis tools span security (vulnerability scanning), code quality (dependencies), and infrastructure (IaC). The domain lacks dedicated "impact assessment" platforms but has strong component analysis tools.

**Recommendation:** **Selective deep analysis** recommended for OWASP Dependency-Track and Blast Radius (Terraform). These offer practical, visual approaches to understanding change impact.

---

## 5. Digital Twin Modelers

### 5.1 IoT Digital Twin Frameworks

#### **Eclipse Ditto** ⭐ RECOMMENDED FOR DEEP ANALYSIS
- **GitHub:** https://github.com/eclipse-ditto/ditto
- **Website:** https://eclipse.dev/ditto/
- **License:** Eclipse Public License 2.0
- **Description:** Open source framework for creating and managing digital twins in IoT. Acts as IoT middleware providing abstraction layer.
- **Activity:** 697 commits by 13 people across 5 repos in last 12 months (active through 2025)
- **Key Features:**
  - Virtual, cloud-based representation of physical devices
  - Well-defined metastructure for IoT/WoT
  - Robust communication infrastructure
  - Client SDKs: Java, JavaScript, Python, Golang
- **Worth Deep Analysis:** ✅ YES - Mature Eclipse project, active development, comprehensive SDKs

#### **Eclipse BaSyx** ⭐ RECOMMENDED FOR DEEP ANALYSIS
- **GitHub:** https://github.com/eclipse-basyx
- **Website:** https://eclipse.dev/basyx/
- **License:** Eclipse Public License 2.0
- **Description:** Open source Industry 4.0 middleware implementing Asset Administration Shell (AAS) standard. "World's most versatile, extensible, feature-packed open source software for AAS."
- **Activity:** Active development through September 2025
- **Key Features:**
  - AAS Version 3 compliant
  - Multi-language SDKs: Java V2, Python, TypeScript, .NET, Rust
  - Real-time data exchange between physical and digital
  - OPC UA and MQTT protocol support
  - Manufacturing/industrial focus
- **Worth Deep Analysis:** ✅ YES - Industry standard (AAS), multi-language support, strong industrial adoption

#### **FIWARE**
- **GitHub:** https://github.com/Fiware
- **Website:** https://www.fiware.org/
- **License:** AGPL-3.0 (Context Broker and various components)
- **Description:** Open source framework for smart solutions with digital twin capabilities. Used in 200+ cities worldwide.
- **Activity:** Active development through August 2025 (spec v1.9.1 July 2025)
- **Key Features:**
  - 84+ Generic Enablers (building blocks)
  - Context Broker (mandatory core component)
  - IoT Agents for protocol translation
  - NGSI context information protocol
  - Smart city focus (Bologna, Antwerp examples)
- **Worth Deep Analysis:** ✅ YES - Massive real-world deployment, comprehensive ecosystem, smart city proven

### 5.2 Next-Generation Digital Twin Platforms

#### **OpenTwins** ⭐ RECOMMENDED FOR DEEP ANALYSIS
- **Research:** ScienceDirect publications (2023, March 2025)
- **License:** Open source
- **Description:** Next-gen platform for 3D-IoT-AI-powered digital twins. Easily develop and orchestrate twins with 3D visualizations, IoT data streams, and real-time ML predictions.
- **Activity:** Recent academic publications through March 2025
- **Key Features:**
  - Distributed digital twins across infrastructures
  - Seamless multi-instance integration
  - 3D-connected visualizations
  - Real-time machine learning
- **Worth Deep Analysis:** ✅ YES - Next-generation capabilities, research-backed, distributed architecture

### 5.3 Azure Digital Twins (Partial Open Source)

#### **Azure Digital Twins Components**
- **DTDL (Digital Twins Definition Language):**
  - **GitHub:** https://github.com/Azure/opendigitaltwins-dtdl
  - **License:** MIT
  - **Description:** Language for describing models and interfaces for IoT digital twins. Open to community collaboration.

- **Digital Twins Explorer:**
  - **GitHub:** https://github.com/Azure-Samples/digital-twins-explorer
  - **License:** MIT
  - **Description:** Web application for visualizing, creating, editing, and diagnosing Azure Digital Twins graphs

- **RealEstateCore Ontology:**
  - **GitHub:** https://github.com/Azure/opendigitaltwins-building
  - **License:** MIT
  - **Description:** DTDL-based ontology for real estate industry

**Worth Deep Analysis:** ⚠️ MAYBE - DTDL is an interesting open standard, but Azure DT SDKs are for proprietary cloud service. Limited value unless Azure ecosystem is primary interest.

### 5.4 Digital Twin Research Summary

A 2024 survey analyzed 14 open-source digital twin frameworks across 10 dimensions. Four platforms stood out:
- Eclipse Ditto
- Eclipse BaSyx
- Snap4City DT
- White Label DT

SCDT was noted for integrated real-time ML capabilities.

**Recommendation:** **Highly recommended for deep analysis** - Digital twin frameworks are mature, well-documented, and actively developed. Eclipse Ditto, Eclipse BaSyx, FIWARE, and OpenTwins offer distinct approaches (IoT middleware, Industry 4.0, smart cities, next-gen ML) worth exploring.

---

## Overall Recommendations Summary

### ✅ STRONGLY RECOMMEND FOR DEEP ANALYSIS

1. **System Architecture:**
   - Archi (ArchiMate) - Industry standard
   - Structurizr - Architecture-as-code pioneer
   - C4-PlantUML - Accessible, version-control friendly
   - Eclipse SysON - Next-gen SysML v2

2. **Data Lineage:**
   - DataHub - Industry leader, comprehensive
   - OpenMetadata - Fastest growing, modern
   - OpenLineage + Marquez - Emerging standard

3. **Impact Assessment:**
   - OWASP Dependency-Track - Enterprise-ready platform
   - Blast Radius (Terraform Fork) - IaC visualization

4. **Digital Twins:**
   - Eclipse Ditto - Mature IoT middleware
   - Eclipse BaSyx - Industry 4.0 standard (AAS)
   - FIWARE - Smart city proven, massive deployment
   - OpenTwins - Next-gen ML-powered

### ⚠️ MAYBE - SELECTIVE ANALYSIS

1. **System Architecture:**
   - Gaphor - Multi-language support
   - TTool - Verification capabilities
   - Mermaid.js - Documentation-as-code

2. **Data Lineage:**
   - Amundsen - Excellent search UX
   - Apache Atlas - Mature but Hadoop-centric

3. **Impact Assessment:**
   - OWASP Dependency-Check - Vulnerability focus
   - Trivy - Container security
   - SonarQube - Broad code quality platform

4. **FMEA:**
   - Open FMEA - Innovative crowdsourcing approach
   - NASA FMEA Tool - Semi-automated methodology

5. **Digital Twins:**
   - Azure DTDL - Open standard, but Azure-tied

### ❌ SKIP - NOT RECOMMENDED

1. **FMEA Domain Overall** - Weak open source ecosystem, commercial dominance
2. **JRipples** - Unmaintained, narrow scope
3. **JDepend** - Limited to Java packages
4. **FMEAApp forks** - Student/learning projects
5. **FMECAengine** - Specialized domain

---

## Gap Analysis

### Strong Open Source Ecosystems
- **System Architecture:** Mature tools across multiple standards (ArchiMate, C4, SysML)
- **Data Lineage:** Rapid innovation, multiple modern platforms competing
- **Digital Twins:** Excellent coverage from IoT to Industry 4.0 to smart cities

### Weak Open Source Ecosystems
- **FMEA:** Very limited options, mostly commercial tools dominate
- **General Impact Assessment:** Fragmented across security scanning, dependency analysis, and IaC - lacks unified platform

### Emerging Opportunities
- **Architecture-as-code convergence** (Structurizr, C4-PlantUML, Mermaid)
- **Lineage standardization** (OpenLineage gaining traction)
- **AI/ML-powered digital twins** (OpenTwins)
- **SysML v2** adoption (Eclipse SysON positioning for future)

---

## Next Steps

### Immediate Deep Dives (Priority Order)

1. **Eclipse Ditto** - Most mature digital twin framework, active community
2. **DataHub** - Industry-leading data lineage, LinkedIn pedigree
3. **Archi** - Most popular ArchiMate tool, extensible
4. **OpenMetadata** - Fastest growing data catalog, comprehensive
5. **Eclipse BaSyx** - Industry 4.0 standard, multi-language SDKs
6. **Structurizr** - Architecture-as-code pioneer
7. **FIWARE** - Massive real-world deployment data
8. **OpenLineage + Marquez** - Emerging standard worth tracking
9. **OWASP Dependency-Track** - Practical impact assessment platform
10. **Eclipse SysON** - Next-gen SysML v2 investment

### Research Activities

For each deep dive:
- Clone repositories
- Review architecture documentation
- Analyze data models and APIs
- Evaluate extensibility mechanisms
- Assess integration capabilities
- Review community health (issues, PRs, releases)
- Document alignment with UML-Codex vision

### Documentation Structure

Create individual analysis documents:
```
/knowledge-store/code-reviews/specialized-modeling/
├── RESEARCH_SUMMARY.md (this file)
├── architecture-tools/
│   ├── archi-analysis.md
│   ├── structurizr-analysis.md
│   ├── c4-plantuml-analysis.md
│   └── syson-analysis.md
├── data-lineage/
│   ├── datahub-analysis.md
│   ├── openmetadata-analysis.md
│   └── openlineage-marquez-analysis.md
├── digital-twins/
│   ├── eclipse-ditto-analysis.md
│   ├── eclipse-basyx-analysis.md
│   ├── fiware-analysis.md
│   └── opentwins-analysis.md
└── impact-assessment/
    ├── dependency-track-analysis.md
    └── blast-radius-analysis.md
```

---

**Research Completed:** November 8, 2025
**Total Tools Identified:** 30+
**Recommended for Deep Analysis:** 14
**Next Review Date:** Q1 2026 (reassess landscape evolution)
