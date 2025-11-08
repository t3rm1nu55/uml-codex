# Celonis - Process Mining and Execution Management Platform

**Research Date:** 2025-11-08
**Focus:** Open Source Repositories and Components

## Executive Summary

Celonis is a commercial process mining and execution management platform with **limited open source presence**. While Celonis maintains a GitHub organization with several open source tools and SDKs, their core process mining platform and Process Query Language (PQL) are proprietary commercial software.

**Key Finding:** For open source process mining analysis, **PM4Py** and **Apromore Core** are recommended alternatives with more substantial open source codebases.

---

## 1. Celonis Open Source Repositories

### Official GitHub Organization
- **Organization:** [github.com/celonis](https://github.com/celonis)
- **Total Public Repositories:** 6 (plus 1 additional Kafka connector)
- **License Types:** MIT, Apache 2.0, LGPL

### Key Repositories

#### 1.1 kafka-ems-connector
- **Repository:** [github.com/celonis/kafka-ems-connector](https://github.com/celonis/kafka-ems-connector)
- **Language:** Scala (sbt 2.13)
- **License:** Apache 2.0
- **Stars:** 43 | **Forks:** 5
- **Latest Release:** v1.8.8
- **Description:** Kafka Connect sink connector allowing data from Apache Kafka to be uploaded to Celonis EMS for real-time process mining
- **Requirements:** Apache Kafka 2.3+, Kafka Connect cluster
- **Use Case:** Real-time data streaming from transactional systems (Oracle, SAP, Salesforce, Workday)
- **Analysis Priority:** ⭐⭐⭐ **HIGH** - Core integration component for real-time process mining

#### 1.2 pycelonis-examples
- **Repository:** [github.com/celonis/pycelonis-examples](https://github.com/celonis/pycelonis-examples)
- **Language:** Jupyter Notebook
- **Stars:** 28 | **Forks:** 20
- **Description:** Demo notebooks covering popular functionalities and use cases of PyCelonis
- **Documentation:** [celonis.github.io/pycelonis](https://celonis.github.io/pycelonis/)
- **Note:** Contains examples only; the actual PyCelonis library is distributed via PyPI (proprietary)
- **Analysis Priority:** ⭐⭐⭐ **HIGH** - Shows API usage patterns and integration approaches

#### 1.3 content-cli
- **Repository:** [github.com/celonis/content-cli](https://github.com/celonis/content-cli)
- **Language:** TypeScript
- **License:** MIT
- **Stars:** 22
- **Description:** Command line interface tool for managing content in Celonis EMS
- **Use Case:** Content management automation, CI/CD integration
- **Analysis Priority:** ⭐⭐ **MEDIUM** - DevOps tooling, useful for understanding content deployment

#### 1.4 PowerPlatformConnectors
- **Repository:** [github.com/celonis/PowerPlatformConnectors](https://github.com/celonis/PowerPlatformConnectors)
- **Language:** C#
- **License:** MIT
- **Forks:** 1,385 (forked from Microsoft)
- **Description:** Connectors for Microsoft Power Automate, Power Apps, and Azure Logic Apps
- **Analysis Priority:** ⭐ **LOW** - Generic Microsoft connector fork

#### 1.5 pdb-controller
- **Repository:** [github.com/celonis/pdb-controller](https://github.com/celonis/pdb-controller)
- **Language:** Go
- **License:** Apache 2.0
- **Forks:** 13
- **Description:** Kubernetes controller for adding default Pod Disruption Budgets to Deployments
- **Analysis Priority:** ⭐ **LOW** - Infrastructure tooling, not process mining specific

#### 1.6 homcc
- **Repository:** [github.com/celonis/homcc](https://github.com/celonis/homcc)
- **Language:** Python
- **License:** MIT
- **Stars:** 20
- **Description:** Work From Home friendly distcc replacement (distributed C/C++ compilation)
- **Analysis Priority:** ⭐ **LOW** - Build tooling, not process mining specific

#### 1.7 theme-whitelabel
- **Repository:** [github.com/celonis/theme-whitelabel](https://github.com/celonis/theme-whitelabel)
- **Language:** Less
- **License:** Apache 2.0
- **Description:** Brandable status page theme for Sorry™
- **Analysis Priority:** ⭐ **LOW** - UI theming, not core functionality

---

## 2. PyCelonis - Python SDK

### Overview
- **Package:** Available on PyPI as `pycelonis`
- **Documentation:** [celonis.github.io/pycelonis](https://celonis.github.io/pycelonis/)
- **Status:** **Proprietary** (not open source)
- **Installation:** `pip install pycelonis`
- **Latest Version:** 2.6.0

### Capabilities
- Programmatic interaction with Celonis EMS objects:
  - Analyses
  - Workspaces
  - Datamodels
  - Datapools
  - Studio
  - Apps
  - Data Integration
- Execute PQL queries from Python
- Integration with Celonis Machine Learning workbench
- New Beta: SaolaPy DataFrame library for direct data interaction

### Example Usage
Available in `pycelonis-examples` repository with Jupyter notebooks demonstrating:
- Connecting to Celonis
- PQL queries in Python
- Data integration workflows
- Process analytics automation

---

## 3. Celonis PQL (Process Query Language)

### Status: **Proprietary - NOT Open Source**

### Overview
- **Type:** Domain-specific language for process mining queries
- **Syntax:** SQL-inspired but specialized for process data
- **Operators:** 150+ operators covering:
  - Process-specific functions
  - Machine learning algorithms
  - Mathematical operations
  - Temporal logic

### Important Distinction
⚠️ **WARNING:** There is a separate, unrelated open source project also called "PQL" at [github.com/processquerying/PQL](https://github.com/processquerying/PQL). This is **NOT** Celonis PQL. It's a different process query language based on temporal logic.

### Documentation
- Official: [docs.celonis.com/en/pql---process-query-language.html](https://docs.celonis.com/en/pql---process-query-language.html)
- Research Paper: "Celonis PQL: A Query Language for Process Mining" (SpringerLink)
- Training: PQL Microlearning courses (free, on Celonis platform)

---

## 4. Alternative Organizations

### Related Celonis GitHub Organizations
- **celonis-content** - [github.com/celonis-content](https://github.com/celonis-content)
- **celonis-gmbh** - [github.com/celonis-gmbh](https://github.com/celonis-gmbh)

Note: These may contain additional repositories beyond the main organization.

---

## 5. Open Source Process Mining Alternatives

Since Celonis has limited open source offerings, here are the leading open source process mining platforms:

### 5.1 PM4Py (⭐⭐⭐⭐⭐ HIGHLY RECOMMENDED)

#### Overview
- **Repository:** [github.com/process-intelligence-solutions/pm4py](https://github.com/process-intelligence-solutions/pm4py)
- **Language:** Python
- **License:** GNU Affero General Public License v3 (AGPL-3.0)
- **Commercial License:** Available for closed-source use
- **Maintainer:** Process Intelligence Solutions GmbH (Fraunhofer FIT spin-off)

#### Statistics
- **Downloads:**
  - Daily: 4,512
  - Weekly: 37,522
  - Monthly: 102,017
- **Python Support:** 3.9.x through 3.14.x

#### Capabilities
- State-of-the-art process mining algorithms
- Process discovery
- Conformance checking
- Performance analysis
- Event log analysis
- Machine learning integration
- Academic and industry-ready

#### Documentation
- Website: [processintelligence.solutions/pm4py](https://processintelligence.solutions/pm4py/)
- PyPI: [pypi.org/project/pm4py](https://pypi.org/project/pm4py/)

#### Analysis Priority
⭐⭐⭐⭐⭐ **CRITICAL** - Most active open source process mining library, excellent for comparative analysis

---

### 5.2 Apromore Core (⭐⭐⭐⭐ RECOMMENDED)

#### Overview
- **Repository:** [github.com/apromore/ApromoreCore](https://github.com/apromore/ApromoreCore)
- **Organization:** [github.com/apromore](https://github.com/apromore)
- **License:** GNU Lesser General Public License v3 (LGPL-3.0)
- **Commercial:** Apromore Enterprise Edition available

#### Features
- Apromore Portal for storing/sharing models and logs
- User and group management
- Process Discoverer for process maps and BPMN models
- Event log importer (XES, CSV, etc.)
- BPMN process model editor
- Predictive process analytics

#### Technical Details
- **Platform:** Linux Ubuntu 20.04, Windows 10/WS2016/WS2019, Mac OSX 10.8+
- **Runtime:** Java SE 11
- **Deployment:** Docker containerized image available

#### Analysis Priority
⭐⭐⭐⭐ **HIGH** - Full-featured platform with academic backing, good for enterprise-level analysis

---

### 5.3 ProM (Process Mining Framework)

#### Overview
- **Official Site:** [promtools.org](https://promtools.org/)
- **Download:** SourceForge and official website
- **License:**
  - ProM 6 Core: GNU Public License (GPL)
  - Plugins: Lesser GNU Public License (LGPL)
- **Language:** Java (platform-independent)

#### Important Note
⚠️ **Not on GitHub:** Source code available via SVN at `https://svn.win.tue.nl/repos/prom/Framework/trunk` (Technical University of Eindhoven)

#### Features
- De facto standard in academic process mining
- Extensible plugin architecture
- Wide variety of process mining algorithms
- Active research community

#### GitHub Ecosystem
While ProM core is not on GitHub, many ProM plugins and extensions are:
- Various process mining tools
- ProM plugin implementations
- Research prototypes

#### Analysis Priority
⭐⭐⭐ **MEDIUM-HIGH** - Academic standard but not on GitHub, requires SVN access

---

## 6. Recommended Repositories for Analysis

### Priority 1: Critical Analysis
1. **PM4Py** - [github.com/process-intelligence-solutions/pm4py](https://github.com/process-intelligence-solutions/pm4py)
   - Most comprehensive open source process mining library
   - Active development, industry adoption
   - Direct competitor/alternative to Celonis capabilities

2. **Apromore Core** - [github.com/apromore/ApromoreCore](https://github.com/apromore/ApromoreCore)
   - Full-featured process mining platform
   - Enterprise-ready architecture
   - Academic and commercial backing

### Priority 2: Celonis Integration Components
3. **kafka-ems-connector** - [github.com/celonis/kafka-ems-connector](https://github.com/celonis/kafka-ems-connector)
   - Real-time data streaming architecture
   - Integration patterns for process mining
   - Kafka Connect best practices

4. **pycelonis-examples** - [github.com/celonis/pycelonis-examples](https://github.com/celonis/pycelonis-examples)
   - API usage patterns
   - Process mining workflows
   - Python integration approaches

### Priority 3: Supporting Tools
5. **content-cli** - [github.com/celonis/content-cli](https://github.com/celonis/content-cli)
   - Content management patterns
   - CI/CD for process mining artifacts

---

## 7. Analysis Recommendations

### For UML Code Analysis

#### If focusing on Celonis specifically:
1. **kafka-ems-connector** - Study real-time data ingestion architecture
2. **pycelonis-examples** - Analyze API design and usage patterns
3. **content-cli** - Examine content management and deployment workflows

#### For broader process mining analysis:
1. **PM4Py** (⭐⭐⭐⭐⭐) - Comprehensive open source alternative
   - Process discovery algorithms
   - Event log processing
   - Conformance checking
   - Performance analysis

2. **Apromore Core** (⭐⭐⭐⭐) - Enterprise platform architecture
   - Full application stack
   - Portal and user management
   - Process modeling integration

### UML Modeling Focus Areas

#### From Celonis repositories:
- Real-time event streaming patterns (Kafka connector)
- REST API design for process mining (PyCelonis examples)
- Content versioning and deployment (content-cli)

#### From PM4Py:
- Process mining algorithm architecture
- Event log data structures
- Discovery and conformance checking flows
- Python library organization for complex analytics

#### From Apromore Core:
- Multi-tier process mining platform architecture
- Process model repository design
- User/group management for collaborative process analysis
- BPMN integration patterns

---

## 8. Commercial vs Open Source Comparison

### Celonis (Commercial)
✅ **Strengths:**
- Enterprise-grade execution management system
- Proprietary PQL with 150+ operators
- Real-time process mining capabilities
- SAP and enterprise system integrations
- Commercial support and SLAs

❌ **Open Source Limitations:**
- Core platform is proprietary
- Limited open source contributions
- PQL is closed source
- PyCelonis library is proprietary (only examples open)
- Few repositories for architectural analysis

### PM4Py (Open Source)
✅ **Strengths:**
- Comprehensive algorithm library
- Active development and community
- Academic research integration
- AGPL license (with commercial option)
- Well-documented APIs

### Apromore Core (Open Source)
✅ **Strengths:**
- Full platform stack open source
- Academic backing (QUT, University of Melbourne)
- BPMN integration
- Docker deployment ready
- LGPL license

---

## 9. Additional Resources

### Process Mining Community Resources

#### GitHub Collections
- [github.com/computertechworld/ProcessMining](https://github.com/computertechworld/ProcessMining) - Curated list of open source process mining tools
- [github.com/lisenkovkv/process_mining](https://github.com/lisenkovkv/process_mining) - Process mining knowledge base

#### Academic Resources
- **ProM Tools:** [promtools.org](https://promtools.org/) - Academic standard
- **Process Mining Software Comparison:** [processmining-software.com/tools](https://www.processmining-software.com/tools/)

### Celonis Documentation
- **Official Docs:** [docs.celonis.com](https://docs.celonis.com)
- **PQL Reference:** [docs.celonis.com/en/pql---process-query-language.html](https://docs.celonis.com/en/pql---process-query-language.html)
- **PyCelonis Docs:** [celonis.github.io/pycelonis](https://celonis.github.io/pycelonis/)

---

## 10. Conclusion and Next Steps

### Key Findings

1. **Celonis Open Source Presence: LIMITED**
   - Only 6-7 public repositories
   - Mostly tooling and examples, not core platform
   - Core process mining engine is proprietary

2. **Best Open Source Alternatives:**
   - **PM4Py** - For Python-based process mining
   - **Apromore Core** - For full platform analysis
   - **ProM** - For academic algorithms (requires SVN)

3. **Recommended Analysis Strategy:**
   - Analyze PM4Py for comprehensive process mining architecture
   - Study Apromore Core for enterprise platform patterns
   - Review Celonis connectors for integration approaches
   - Use pycelonis-examples for API design patterns

### Suggested Repository Analysis Order

For **UML Code Architecture Analysis**:

1. **Phase 1:** Core Process Mining
   - PM4Py main repository
   - PM4Py algorithm implementations
   - Event log processing architecture

2. **Phase 2:** Platform Architecture
   - Apromore Core
   - Portal and repository design
   - Multi-user collaboration patterns

3. **Phase 3:** Integration Patterns
   - Celonis kafka-ems-connector
   - Real-time data streaming
   - Celonis content-cli for deployment

4. **Phase 4:** API and Usage
   - pycelonis-examples
   - API design patterns
   - Workflow automation

### Next Actions

- [ ] Clone and analyze PM4Py repository structure
- [ ] Clone and analyze Apromore Core architecture
- [ ] Review Celonis kafka-ems-connector for real-time patterns
- [ ] Study pycelonis-examples for API design insights
- [ ] Compare architectures across all three platforms
- [ ] Extract UML models for process mining domain patterns

---

**Document Status:** Research Complete
**Last Updated:** 2025-11-08
**Analyst Notes:** Celonis has minimal open source presence. Recommend focusing on PM4Py and Apromore Core for substantial open source process mining architecture analysis.
