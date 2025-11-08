# Specialized Modeling Tools - Summary Insights
**Date**: 2025-11-08
**Version**: 1.0
**Status**: Analysis Complete

---

## Executive Summary

This document synthesizes insights from analyzing 6 industry-leading specialized modeling tools across architecture, data lineage, impact assessment, and digital twins.

### Tools Analyzed

| Tool | Domain | License | Stars | Assessment |
|------|--------|---------|-------|------------|
| **Archi** | System Architecture | MIT | 1,100+ | CAN_LIFT |
| **DataHub** | Data Lineage | Apache-2.0 | 11,200+ | CAN_LIFT |
| **OpenMetadata** | Data Catalog/Lineage | Apache-2.0 | 7,900+ | CAN_LIFT |
| **Eclipse Ditto** | Digital Twin (IoT) | EPL-2.0 | N/A | CAN_LIFT |
| **Eclipse BaSyx** | Digital Twin (Industry 4.0) | MIT | N/A | CAN_LIFT |
| **OWASP Dependency-Track** | Impact Assessment | Apache-2.0 | 3,400+ | CAN_LIFT |

**Key Takeaway**: All analyzed tools use permissive licenses - no legal blockers for reuse!

---

## 1. System Architecture Modeling (Archi)

### Core Value for UML-Codex

**Model-View Separation** (Essential Pattern):
```
Semantic Model (IArchimateModel)         Visual Model (IDiagramModel)
├── Elements (concepts)                  ├── Shapes (positions, colors)
├── Relationships (connections)          ├── Connections (bend points)
└── Properties (metadata)                └── Styles (fonts, fills)

Same element appears in multiple diagrams with different visual properties
```

**Direct Application**: UML Class appears in Class Diagram, Sequence Diagram, Component Diagram - each with different layout/styling.

### Metamodel Implementation: EMF-Based

**Archi's Approach**:
- 494KB Ecore metamodel (`archimate.ecore`)
- Code generation produces: Interfaces, Implementations, Factory, Package
- 40+ element types, 20+ relationship types
- Automatic XMI serialization

**For UML-Codex**: Consider Eclipse UML2 (EMF-based UML 2.5.1 implementation) vs custom generation

### Viewpoint Architecture

**ArchiMate Pattern**:
```java
interface IViewpoint {
    String getName();
    String[] getAllowedTypes(); // Elements and relationships
}

class ViewpointManager {
    IViewpoint getViewpoint(String id);
    void setViewpoint(IDiagramModel diagram, IViewpoint vp);
}
```

**UML Application**: Map to diagram types
- Class Diagram viewpoint: Class, Interface, Association, Generalization
- Sequence Diagram viewpoint: Lifeline, Message, ExecutionSpecification
- Activity Diagram viewpoint: Action, ControlFlow, ObjectFlow, ActivityPartition

### Eclipse GEF Visual Editor

**MVC Pattern**:
- **Model**: EMF objects (IArchimateElement)
- **View**: Draw2D Figures (visual rendering)
- **Controller**: EditParts (handle user interaction)

**Command Pattern**: All modifications = undoable commands

**Not Recommended**: Eclipse GEF is legacy. Learn patterns but implement in modern framework (React + Canvas, JavaFX, web-based).

### Key Recommendations

✅ **Adopt**:
- Model-View separation
- Viewpoint/Diagram type filtering
- Command pattern for undo/redo
- Factory pattern from metamodel
- Extension points for plugins
- Multiple serialization formats (XMI, JSON, CSV)

⚠️ **Consider**:
- EMF for metamodel (powerful but Eclipse-tied)
- Code generation vs manual (UML scale suggests generation)

❌ **Avoid**:
- Eclipse RCP (desktop-only, heavy, declining)
- GEF 3.x (legacy technology)

---

## 2. Data Lineage Modeling (DataHub + OpenMetadata)

### Metadata Graph Model

**Common Pattern Across Both Tools**:
```
Entities (Nodes)          Aspects/Properties (Attributes)          Relationships (Edges)
├── Dataset              ├── Schema                                ├── DownstreamOf
├── Dashboard            ├── Ownership                             ├── OwnedBy
├── Pipeline             ├── Tags                                  ├── Contains
└── User                 └── Lineage                              └── Consumes
```

**DataHub URN Example**: `urn:li:dataset:(platform,name,fabric)`
**OpenMetadata FQN Example**: `service.database.schema.table`

**For UML-Codex**: Similar structure for UML relationships
```
UML Entities             UML Properties                            UML Relationships
├── Class                ├── Attributes                            ├── Association
├── Interface            ├── Operations                            ├── Generalization
├── Package              ├── Stereotypes                           ├── Realization
└── Component            └── Tagged Values                         └── Dependency
```

### Lineage Tracking

**Multi-Granularity Pattern**:
- **Dataset-level**: Table A → Table B → Table C
- **Column-level**: Column A.x → Column B.y → Column C.z
- **Pipeline-level**: Airflow DAG → Spark Job → dbt Transform

**Storage**: Neo4j (DataHub), MongoDB + Graph API (OpenMetadata)

**For UML-Codex**:
- **Model-level**: Package dependencies
- **Element-level**: Class uses Interface, Component realizes Interface
- **Trace-level**: Requirement → Class → Test Case

### JSON Schema-Based Modeling (OpenMetadata)

**Excellent Pattern**:
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Table",
  "type": "object",
  "properties": {
    "id": {"type": "string", "format": "uuid"},
    "name": {"type": "string"},
    "fullyQualifiedName": {"type": "string"},
    "columns": {
      "type": "array",
      "items": {"$ref": "column.json"}
    }
  }
}
```

**Benefits**:
- Type safety
- Validation
- Auto-generated documentation
- Language-agnostic

**For UML-Codex**: Consider JSON Schema for UML metamodel representation alongside XMI

### Connector Framework (84+ in OpenMetadata, 100+ in DataHub)

**Architecture**:
```
Source (Database/BI/Pipeline) → Extractor (metadata) →
Transformer (normalize) → Sink (publish to catalog)
```

**For UML-Codex**: Import/Export framework
```
Source (EA/MagicDraw/Papyrus/Rhapsody) → Parser (XMI/custom) →
Transformer (normalize to UML 2.5.1) → Validator → Repository
```

### Collaboration Features (OpenMetadata)

**Thread Model**:
- Conversations (questions/answers)
- Tasks (assigned work with status)
- Announcements (broadcasts)
- Chatbot (AI-assisted Q&A)

**For UML-Codex**: Model review conversations
```
Thread on Class Diagram:
├── User A: "Should Customer have billing address?"
├── User B: "Yes, add as association to Address"
└── Task: Add Customer-Address association [DONE]
```

### Key Recommendations

✅ **Adopt**:
- Graph-based metadata model
- Multi-granularity lineage (model/package/element)
- JSON Schema for metamodel (alongside XMI)
- Connector plugin architecture
- Collaboration via threaded conversations
- Elasticsearch for full-text search

⚠️ **Consider**:
- Neo4j vs embedded graph (complexity vs features)
- Full platform vs selective reuse

❌ **Avoid**:
- Over-engineering for small teams
- Three-language stack (Java/Python/TypeScript) unless needed

---

## 3. Digital Twin Modeling (Eclipse Ditto + Eclipse BaSyx)

### Core Digital Twin Patterns

**Thing/Feature Structure (Ditto)**:
```json
{
  "thingId": "namespace:thing-name",
  "policyId": "namespace:policy-name",
  "attributes": {}, // Static properties
  "features": {    // Dynamic capabilities
    "temperature": {
      "properties": {"value": 23.5, "unit": "°C"},
      "desiredProperties": {"value": 22.0}
    }
  }
}
```

**AAS Structure (BaSyx - Industry 4.0)**:
```
Asset (physical entity)
└── Asset Administration Shell (digital twin)
    └── Submodels (aspects)
        ├── TechnicalData
        ├── OperationalData
        └── Documentation
            └── SubmodelElements
                ├── Property
                ├── File
                └── Operation
```

**For UML-Codex**: Similar hierarchical structure
```
System (real system)
└── UML Model (digital representation)
    └── Packages (aspects)
        ├── Domain Model
        ├── Architecture
        └── Behavior
            └── Elements
                ├── Class
                ├── Diagram
                └── Constraint
```

### State Synchronization Patterns

**Twin vs Live Channels (Ditto)**:
- **Twin Channel**: Persisted state, always available, eventually consistent
- **Live Channel**: Real-time device with timeout, no persistence

**For UML-Codex**: Model editing
- **Persisted Model**: Database, version controlled, collaborative
- **Live Editing**: WebSocket, real-time cursors, operational transform

### Event-Driven Architecture

**CQRS + Event Sourcing (Both Tools)**:
- **Commands**: Create, Modify, Delete (write path)
- **Queries**: Retrieve, Search (read path)
- **Events**: All changes stored as immutable events

**For UML-Codex**: Model operations as events
```
ModelCreatedEvent
ElementAddedEvent
AttributeModifiedEvent
DiagramLayoutChangedEvent
```

### Policy-Based Authorization (Ditto)

**Fine-Grained Permissions**:
```json
{
  "entries": {
    "user:alice": {
      "subjects": {"type": "user"},
      "resources": {
        "thing:/features/temperature": {
          "grant": ["READ"],
          "revoke": ["WRITE"]
        }
      }
    }
  }
}
```

**For UML-Codex**: Element-level permissions
```json
{
  "team:architects": {
    "resources": {
      "model:/packages/architecture": {
        "grant": ["READ", "WRITE"]
      },
      "model:/packages/implementation": {
        "grant": ["READ"]
      }
    }
  }
}
```

### Multi-Protocol Support

**Ditto Supports**:
- HTTP/REST (synchronous)
- WebSocket (bidirectional)
- MQTT (pub/sub, IoT)
- AMQP (messaging)
- Kafka (event streaming)
- SSE (server push)

**For UML-Codex**: API diversity
- REST (CRUD operations)
- GraphQL (flexible queries)
- WebSocket (real-time editing)
- gRPC (performance-critical operations)

### W3C Web of Things Integration (Ditto)

**Thing Models as Templates**:
- Define interfaces/capabilities
- JSON-LD for semantic descriptions
- Schema validation
- Ontology support (QUDT, SAREF)

**For UML-Codex**: UML Profiles as "Thing Models"
```
UML Profile (template)
└── Stereotypes (capabilities)
    ├── <<Entity>> (persistence)
    ├── <<Service>> (business logic)
    └── Tagged Values (properties)
```

### Semantic Modeling (BaSyx)

**ConceptDescription Pattern**:
```
ConceptDescription (semantic definition)
├── Identifier (globally unique)
├── preferredName (human-readable)
├── dataSpecification (ECLASS/IEC CDD)
└── embeddedDataSpecifications (properties)
```

**For UML-Codex**: Reference ontologies
- UML: http://www.omg.org/spec/UML/
- SysML: http://www.omg.org/spec/SysML/
- BPMN: http://www.omg.org/spec/BPMN/

### Key Recommendations

✅ **Adopt**:
- Hierarchical twin structure (Thing/Feature or AAS/Submodel)
- CQRS + Event Sourcing
- Policy-based fine-grained authorization
- Multi-protocol APIs
- State synchronization patterns
- Semantic modeling with ontologies

⚠️ **Consider**:
- Microservices architecture (complexity vs scalability)
- Event sourcing (storage overhead vs audit trail)

❌ **Avoid**:
- Over-architecting for simple use cases
- Premature optimization for IoT scale

---

## 4. Impact Assessment (OWASP Dependency-Track)

### SBOM (Software Bill of Materials) Processing

**CycloneDX & SPDX Support**:
```
SBOM Upload → Validation → Parsing →
Component Extraction → Deduplication →
Vulnerability Matching → Risk Scoring
```

**For UML-Codex**: Model Bills of Materials (MBOM)
```
Model Upload (XMI) → Validation → Parsing →
Element Extraction → Deduplication →
Constraint Checking → Quality Scoring
```

### Vulnerability Matching Algorithm

**Multi-Source Aggregation**:
```
Component (name, version, PURL, CPE)
↓
Query: NVD, GitHub Advisories, OSS Index, Snyk, Trivy, OSV
↓
Version Range Matching
↓
CVE Assignment + CVSS Scoring
```

**For UML-Codex**: Anti-Pattern Detection
```
UML Element (type, stereotype, context)
↓
Query: Anti-pattern database, Code smells, Best practices
↓
Pattern Matching (structural + semantic)
↓
Issue Assignment + Severity Scoring
```

### Risk Scoring Methodology

**Weighted Severity**:
```
CRITICAL: 10 points
HIGH: 5 points
MEDIUM: 3 points
LOW: 1 point

Project Risk = Σ(vulnerability_count × severity_weight)
```

**For UML-Codex**: Model Quality Scoring
```
BLOCKER: 10 points (violates UML spec)
CRITICAL: 5 points (anti-pattern)
MAJOR: 3 points (code smell)
MINOR: 1 point (style issue)

Model Quality = 100 - Σ(issue_count × severity_weight)
```

### Dependency Graph

**JSON-Based Storage**:
```json
{
  "directDependencies": [
    {
      "uuid": "component-uuid",
      "ref": "pkg:maven/group/artifact@version"
    }
  ]
}
```

**For UML-Codex**: Element Dependencies
```json
{
  "dependencies": [
    {
      "element": "class-uuid",
      "type": "uses",
      "target": "interface-uuid"
    }
  ]
}
```

### Policy Engine

**Pluggable Evaluator Pattern**:
```java
interface PolicyEvaluator {
    boolean evaluate(Component component, PolicyCondition condition);
}

class VersionEvaluator implements PolicyEvaluator {
    // Check if version matches condition
}

class SeverityEvaluator implements PolicyEvaluator {
    // Check if severity exceeds threshold
}
```

**For UML-Codex**: Validation Rules
```python
class ValidationRule(ABC):
    @abstractmethod
    def evaluate(self, element: UMLElement) -> ValidationResult:
        pass

class NamingConventionRule(ValidationRule):
    def evaluate(self, element: UMLElement) -> ValidationResult:
        # Check naming conventions
        pass
```

### Blast Radius Analysis

**Affected Project Tracking**:
```
Vulnerable Component
├── affectedProjectCount: 42
├── affectedActiveProjectCount: 35
└── affectedInactiveProjectCount: 7
```

**For UML-Codex**: Change Impact Analysis
```
Modified Element
├── affectedDiagramCount: 8
├── affectedClassCount: 15
└── affectedTestCount: 23
```

### Key Recommendations

✅ **Adopt**:
- SBOM processing patterns (for MBOM)
- Multi-source validation (spec + best practices + anti-patterns)
- Risk scoring methodology
- Policy engine with pluggable evaluators
- Dependency/impact graph
- Blast radius calculation

⚠️ **Consider**:
- Full platform vs selective patterns
- Event-driven analysis (performance)

❌ **Avoid**:
- Over-complicating for small models
- Real-time scoring for every keystroke (batch instead)

---

## 5. Cross-Cutting Insights

### Metamodel Implementation Approaches

| Tool | Approach | Pros | Cons |
|------|----------|------|------|
| **Archi** | EMF (Ecore → code gen) | Auto serialization, reflection | Eclipse dependency |
| **DataHub** | Pegasus (PDL → code gen) | Type safety, REST.li integration | LinkedIn-specific |
| **OpenMetadata** | JSON Schema | Language-agnostic, validation | Manual Java/TypeScript code |
| **Ditto** | Java classes + JSON | Simple, flexible | No formal metamodel |
| **BaSyx** | AAS4J library | Standards-compliant | Industry 4.0 specific |
| **Dependency-Track** | JDO annotations | ORM integration | Tight coupling to persistence |

**Recommendation for UML-Codex**:
1. **Option A**: Eclipse UML2 (EMF-based, standard UML 2.5.1)
2. **Option B**: JSON Schema + code generation (Pydantic for Python)
3. **Option C**: Custom DSL (Rosetta-like) + code generation

### Event-Driven Architecture

**Consistent Pattern Across Tools**:
```
Event Producer → Event Bus (Kafka) → Event Consumer
├── API (user actions)              ├── Indexer (search)
├── File Upload                     ├── Analyzer (validation)
└── External System                 └── Notifier (alerts)
```

**For UML-Codex**:
```
Model Change Event → Event Bus → Consumers
├── Edit (user)                   ├── Validator
├── Import (XMI)                  ├── Generator
└── Sync (git)                    └── Collaborator
```

### Plugin/Extension Architecture

**Extension Points**:
- Archi: 6 extension points (importers, exporters, UI providers)
- Ditto: Connectivity (custom protocols, payload mappers)
- BaSyx: Custom operations (runtime Java compilation!)
- Dependency-Track: Analyzers, publishers, evaluators
- OpenMetadata: Connectors (84+)

**For UML-Codex**: Extension points
```
Extension Points:
├── Importers (EA, MagicDraw, Papyrus, StarUML)
├── Exporters (XMI, JSON, PlantUML, GraphML)
├── Validators (OCL, custom rules, anti-patterns)
├── Generators (Python, Java, C++, SQL, OpenAPI)
├── Visualizers (custom diagram renderers)
└── Integrations (Jira, Confluence, Git)
```

### Multi-Tenancy Patterns

**Organization Hierarchy**:
```
Organization (OpenMetadata Domains)
└── Teams (with parent-child)
    └── Users (with roles)
        └── Permissions (RBAC)
```

**For UML-Codex**:
```
Organization
└── Projects (hierarchical)
    └── Models (with ownership)
        └── Access Control (element-level)
```

### Search and Query

**Common Stack**:
- **Full-text**: Elasticsearch/OpenSearch
- **Graph traversal**: Neo4j or MongoDB with graph extensions
- **Query language**: RQL (Ditto), GraphQL (DataHub), custom REST (others)

**For UML-Codex**:
- **Full-text**: Search element names, stereotypes, documentation
- **Structural**: OCL-based queries on UML model
- **Visual**: Find elements by diagram appearance

### API Design

**REST + GraphQL Hybrid**:
- **REST**: CRUD operations, standard HTTP verbs
- **GraphQL**: Complex queries, nested data fetching
- **WebSocket**: Real-time updates

**For UML-Codex**:
- **REST**: Model/element CRUD
- **GraphQL**: Complex diagram queries
- **WebSocket**: Collaborative editing
- **gRPC**: Code generation (performance)

---

## 6. Technology Stack Synthesis

### Recommended for UML-Codex

**Backend**:
- **Language**: Python 3.11+ or Java 21+
- **Framework**: FastAPI (Python) or Spring Boot (Java)
- **Database**: PostgreSQL (primary), Redis (cache)
- **Search**: Elasticsearch
- **Events**: Apache Kafka or Redis Streams
- **Metamodel**: EMF (UML2) or Pydantic + JSON Schema

**Frontend**:
- **Language**: TypeScript 5+
- **Framework**: React 18+ or Vue 3+
- **Build**: Vite
- **UI**: Ant Design or Material-UI
- **State**: Zustand or Pinia
- **Diagrams**: ReactFlow or Konva.js
- **Collaboration**: Yjs (CRDT)

**Infrastructure**:
- **Containers**: Docker + Kubernetes
- **API Gateway**: NGINX or Kong
- **Auth**: Keycloak (OAuth2/SAML)
- **Observability**: Prometheus + Grafana + Jaeger

---

## 7. Implementation Priorities

### Phase 1: Core Metamodel (Months 1-3)
**Inspired by**: Archi, OpenMetadata
- UML 2.5.1 metamodel (EMF or Pydantic)
- Model-View separation
- XMI + JSON serialization
- Basic CRUD API

### Phase 2: Visual Editing (Months 3-6)
**Inspired by**: Archi
- Diagram types as viewpoints
- Canvas-based rendering (React + Konva)
- Command pattern for undo/redo
- Layout algorithms

### Phase 3: Lineage & Impact (Months 4-7)
**Inspired by**: DataHub, OpenMetadata, Dependency-Track
- Dependency graph
- Multi-level lineage (package → element)
- Change impact analysis
- Blast radius visualization

### Phase 4: Validation & Quality (Months 5-8)
**Inspired by**: Dependency-Track, OpenMetadata
- OCL constraint evaluation
- Anti-pattern detection
- Quality scoring
- Policy engine

### Phase 5: Collaboration (Months 6-9)
**Inspired by**: OpenMetadata, Ditto
- Real-time co-editing (CRDT)
- Threaded conversations
- Task management
- Notifications

### Phase 6: Code Generation (Months 7-10)
**Inspired by**: REGnosys (prior analysis)
- Multi-language generation
- Template-based approach
- Plugin architecture
- Execution testing

### Phase 7: Digital Twin (Months 9-12)
**Inspired by**: Ditto, BaSyx
- Model as digital twin of system
- Sync with code/runtime
- Event-driven updates
- Multi-protocol access

---

## 8. Key Learnings Summary

### Architecture Patterns
✅ Model-View separation (Archi)
✅ CQRS + Event Sourcing (Ditto, BaSyx)
✅ Microservices with event bus (all tools)
✅ Plugin/extension points (all tools)
✅ Multi-tenancy with RBAC (OpenMetadata, Ditto)

### Metamodel Patterns
✅ EMF-based code generation (Archi, UML2)
✅ JSON Schema-based (OpenMetadata)
✅ Aspect-oriented metadata (DataHub)
✅ Hierarchical structures (BaSyx AAS)
✅ Viewpoint filtering (Archi)

### Data Patterns
✅ Graph-based relationships (DataHub, OpenMetadata)
✅ Multi-granularity lineage (both lineage tools)
✅ Dependency tracking (Dependency-Track)
✅ Event sourcing (Ditto)
✅ Eventual consistency (Ditto, distributed systems)

### API Patterns
✅ REST + GraphQL hybrid (DataHub)
✅ WebSocket for real-time (Ditto, collaboration)
✅ Multi-protocol support (Ditto, BaSyx)
✅ OpenAPI documentation (all tools)
✅ JWT authentication (all tools)

### Quality Patterns
✅ Multi-source validation (Dependency-Track)
✅ Policy engine with evaluators (Dependency-Track, OpenMetadata)
✅ Risk scoring (Dependency-Track)
✅ Test definitions + cases (OpenMetadata)
✅ Pluggable analyzers (Dependency-Track)

---

## 9. Unique Innovations to Consider

**From Archi**:
- Sketch canvas (free-form diagrams)
- HTML reports with customizable templates
- CSV import/export (bulk operations)

**From DataHub**:
- Aspect-oriented metadata (small, versioned units)
- Pegasus Data Language (schema language)
- GraphQL schema stitching

**From OpenMetadata**:
- JSON Schema-driven UI generation
- 84+ pre-built connectors
- Data quality dimensions
- Conversation threads on entities

**From Ditto**:
- Twin vs Live channels
- Weak acknowledgements
- Placeholder system for dynamic routing
- Historical API (point-in-time queries)

**From BaSyx**:
- Runtime Java compilation for operations
- Semantic concept descriptions
- Multi-level AAS nesting
- AASX package format

**From Dependency-Track**:
- VEX (Vulnerability Exploitability Exchange)
- EPSS (Exploit Prediction Scoring)
- Multi-source aggregation
- Inherited risk scores

---

## 10. Conclusion

### All Tools Are Highly Reusable

**Licenses**:
- MIT: Archi, BaSyx (most permissive)
- Apache-2.0: DataHub, OpenMetadata, Dependency-Track (permissive)
- EPL-2.0: Ditto (weak copyleft, library-friendly)

**No Blockers**: All licenses permit commercial and open-source reuse.

### Recommended Adoption Strategy

**Tier 1 (High Priority Patterns)**:
1. Model-View separation (Archi)
2. Event-driven architecture (all)
3. Plugin extensibility (all)
4. Policy engine (Dependency-Track, OpenMetadata)
5. Multi-granularity lineage (DataHub, OpenMetadata)

**Tier 2 (Medium Priority Patterns)**:
1. CQRS + Event Sourcing (Ditto, BaSyx)
2. Microservices architecture (all)
3. JSON Schema modeling (OpenMetadata)
4. Semantic modeling (BaSyx, Ditto)
5. Multi-protocol APIs (Ditto, BaSyx)

**Tier 3 (Low Priority / Future)**:
1. Digital twin state sync (Ditto)
2. Industry 4.0 AAS compliance (BaSyx)
3. W3C WoT integration (Ditto)
4. SBOM processing (Dependency-Track)

### Next Steps

1. **Prototype Phase 1**: Core metamodel with EMF or Pydantic
2. **Evaluate**: Eclipse UML2 vs custom implementation
3. **Design**: Plugin architecture for extensibility
4. **Implement**: Model-View separation for multi-diagram support
5. **Test**: Validate patterns with UML use cases

---

**End of Summary**

This document provides strategic guidance for building UML-Codex by learning from 6 industry-leading specialized modeling tools. All insights integrate with the existing knowledge base from workflow engines, process mining, and domain modeling platforms.

Total repositories analyzed: **19** (13 prior + 6 specialized)
Total insights pages: **400+**
Total design patterns: **150+**
Total requirements: **150+**
