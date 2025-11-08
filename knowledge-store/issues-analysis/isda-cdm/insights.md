# FINOS Common Domain Model - Issues Analysis Insights

**Repository**: finos/common-domain-model
**Analysis Date**: 2025-11-08
**Issues Analyzed**: 127 open issues + recent closed issues
**Current Milestone**: Major Release 7.0 (due 2026-02-28, 67% complete)

## Executive Summary

The FINOS Common Domain Model (CDM) repository demonstrates mature governance for a production-grade financial industry standard. With 127 open issues and active development across multiple working groups, the project coordinates backward-incompatible changes through major releases while maintaining parallel minor releases. Key challenges include state management fragility, qualification function misalignment, and the complexity of integrating legacy standards like FpML. The repository provides valuable lessons for UML-Codex on domain model evolution, governance patterns, and the relationship between modeling languages and domain models.

---

## 1. Domain Modeling Challenges

### 1.1 State Management and Identity

**Challenge**: Nested object design can break global key stability when objects represent mutable relationships rather than immutable identity.

**Evidence**:
- Issue #4041: Person class nested within party class causes parent party's global key to change when personnel assignments shift
- Problem occurs because personnel assignments are workflow-level roles, not identity attributes
- Workaround via `partyChangeInstruction` on every workflow step impractical at transaction scale
- Related to Issue #3319 on entity identifier types and party-to-person relationships

**Resolution Pattern**:
- Proposed split: counterparty persons (static, part of party identity) vs role-holding persons (dynamic, workflow-level associations)
- Shift from nested composition to association where person points to party rather than vice versa
- Leverage existing but unused `RelatedParty` concept as potential framework

**UML-Codex Implications**:
- Distinguish between identity attributes vs relationship attributes in domain models
- Implement global key stability analysis to detect when nested objects could affect parent identity
- Provide modeling guidance: composition for identity, association for roles
- Support warnings when mutable nested objects threaten identity stability

### 1.2 Component Granularity

**Challenge**: Insufficient granularity in primitive events and functions leads to ambiguity and multiple downstream issues.

**Evidence**:
- Issue #3816: QuantityChange primitive event handles both price and quantity changes ambiguously
- Single function trying to serve multiple purposes creates confusion
- Six related sub-issues tracked as consequences
- Discussion suggests separating price and quantity updates into distinct functions

**Resolution Pattern**:
- Three-step investigation: assess current implementation, identify adjustments, or develop alternatives
- Convert from Java implementation to Rune DSL for better expressiveness
- Increase granularity in price/quantity attributes within payouts
- Schedule deep-dive session for holistic review

**UML-Codex Implications**:
- Detect ambiguous multi-purpose components through analysis
- Recommend separation of concerns when single component handles multiple dimensions
- Track downstream issues caused by granularity problems
- Support refactoring tools to split overly broad components

### 1.3 Qualification Function Alignment

**Challenge**: Qualification logic easily becomes misaligned with domain model evolution, causing incorrect event and product classification.

**Evidence**:
- Issue #3798: Termination qualification logic doesn't align with expected changes
- Issue #3987: Partial termination not qualified properly
- Issue #4122: Unclear what Qualify_SecuritySettlement actually qualifies
- Issue #4083: Need to update Qualify_SecurityLending function
- All marked for Major Release 7.0 with backward-incompatible label

**Pattern**:
- Qualification functions are separate from model definitions
- Model changes don't automatically update qualification logic
- Functions need explicit review and updates with each major version
- Testing required to ensure correct classification

**UML-Codex Implications**:
- Track qualification functions alongside domain model definitions
- Detect when model changes might break qualification logic
- Support test cases for qualification function correctness
- Enable qualification function versioning synchronized with model versions
- Provide visualization showing what qualifications apply to what types

### 1.4 Observation and Valuation Restrictions

**Challenge**: ObservationTerms and ValuationTerms restricted to particular payout types rather than applying generically, misaligning with ISDA Definitions standards.

**Evidence**:
- Issue #3748: FRAGMOS major refactoring proposal for observation improvements
- Current design forces observation into Reset or Transfer events rather than standalone
- Lack of consistency and overlaps in existing components (reset, resetOrigin, resetHistory)
- Missing "resolving method" component for price calculations
- Observable type lacks attributes for recording observation-derived dated values

**Resolution Pattern**:
- Move ObservationTerms and ValuationTerms to PayoutBase for generic applicability
- Create new PriceObservation type with ObservationPriceValue and PriceResolvingMethod
- Enable standalone observation events composable with other event types
- Add optional priceObservation attribute to Observable using existing PriceSchedule type

**Governance Challenge**:
- CRWG acknowledges need for dedicated governance separate from standing committees
- Steering WG notes complexity, consequential nature, needs adequate resourcing and task force
- Currently parked pending prioritization and resource allocation
- Demonstrates how complex structural changes can stall without dedicated support

**UML-Codex Implications**:
- Support generic vs specific component placement decisions
- Enable detection of artificial restrictions in applicability
- Flag complex changes requiring special governance
- Provide impact analysis tools for cross-cutting architectural changes
- Support resource planning and task force coordination

---

## 2. Domain Model Evolution Patterns

### 2.1 Coordinated Major Releases

**Pattern**: Backward-incompatible changes grouped together into major releases with clear milestone targeting.

**Evidence**:
- Major Release 7.0: 70 total issues (47 closed, 23 open) all backward-incompatible
- Due date: 2026-02-28 (4-month window)
- Clear labeling: "backward-incompatible; must be on Steering WG roadmap; 2 maintainers to approve"
- Parallel minor releases (5.29, 6.11) handle non-breaking changes
- 67% completion rate provides progress visibility

**Benefits**:
- Provides clear migration path for adopters
- Reduces upgrade complexity by consolidating breaking changes
- Enables comprehensive testing of interactions between changes
- Allows adequate time for community review and preparation

**UML-Codex Implications**:
- Implement versioning strategy supporting major/minor release tracks
- Enable backward-compatibility tracking and analysis
- Build tooling to identify breaking changes automatically
- Support migration path generation for major version upgrades
- Enable grouping related breaking changes into coordinated releases

### 2.2 Legacy Code Consolidation

**Pattern**: Continuous effort to merge legacy types, retire old mappings, and simplify model structure.

**Evidence**:
- Issue #4049: Merging Legacy Threshold & MTA types with Threshold/MTA (closed)
- Issue #4051: Merging Valuation/Calculation Agent types (closed)
- Issue #4053: Remaining IM/VM clauses updates (closed)
- Issue #4072: Updating IM/VM & Legacy Credit Support clauses (closed)
- Issue #3949: Consolidation of enums in legacy CSA clauses (closed)
- Issue #4116: Retire non-FpML legacy mappings (open)
- Issue #4115: Migrate legacy CreateIQ synonym mappings (open)

**Rationale**:
- Reduce duplication in model
- Simplify validation logic
- Improve data integrity
- Ease maintenance burden

**Approach**:
- Identify duplicate or near-duplicate types
- Merge into unified structure
- Update all references
- Mark as backward-incompatible for major release
- Retire in coordinated fashion

**UML-Codex Implications**:
- Detect duplicate or similar model elements
- Suggest consolidation opportunities
- Support refactoring with automated reference updates
- Track technical debt related to legacy structures
- Enable migration path documentation

### 2.3 Composition Over Core Integration

**Pattern**: New capabilities added through modular composition rather than core model modification.

**Evidence**:
- Issue #3333: Tokenized assets via TokenisationDetails component (closed, approved)
  - Modular approach provides flexibility to apply to any asset type
  - Composition allows future enhancements without altering core models
  - Distinguishes tokenization features from base asset properties
- Issue #3432: Property and physical risk as attached specifications (closed, approved)
  - Risk attributes attach as contained specifications similar to collateral
  - Property as asset type with risk specifications rather than standalone
  - Can serve as underlying collateral to existing Loan instruments

**Debate Points**:
- Tension between core integration vs external extension
- Concerns about model bloat and reference data accumulation
- Counter-argument: fundamental asset classes deserve core placement
- Resolution: composition patterns allow extension without architectural upheaval

**UML-Codex Implications**:
- Support modular composition patterns for domain extensions
- Enable analysis of core vs external extension tradeoffs
- Provide impact analysis for model bloat concerns
- Support both composition and inheritance strategies
- Track extension approval through governance workflow

### 2.4 Systematic Improvement Campaigns

**Pattern**: Contributors submit coordinated sets of related improvements addressing structural deficiencies.

**Evidence**:
- FRAGMOS initiative: 17+ related issues covering interconnected improvements
  - Observable & Pricing enhancements
  - Barrier types (KnockBarrier, CapFloorBarrier, Trigger refactoring)
  - Quantity framework (BaseQuantity replacing ResolvablePriceQuantity)
  - Payout structure (CorePayoutTerms variants)
  - Transfer enrichment (NotionalQuantity, Price, SSI attributes)
  - Major observation improvements (Issue #3748)
- Securities lending expansion across multiple dimensions
  - Collateral management (Issue #3896)
  - Fee structures (Issue #3871)
  - Transfer types (Issue #3870)
  - Rate changes (Issue #3640)
  - Settlement states (Issue #4071)
  - Qualification functions (Issue #4083)
  - Visualizations (Issue #4139)

**Benefits**:
- Holistic view of problem domain
- Coherent improvement across related areas
- Clear attribution and ownership
- Easier to track dependencies and progress

**UML-Codex Implications**:
- Support tagging issues by contributor/initiative
- Enable dependency tracking across related improvements
- Provide campaign/epic management capabilities
- Support phased rollout of coordinated changes
- Track approval status across working groups

---

## 3. Governance and Collaboration Patterns

### 3.1 Multi-Working-Group Structure

**Structure**:
- Steering Working Group (SWG): Strategic direction, complex change approval
- Technical Architecture Working Group (TAWG): Technical decisions, DSL governance
- Contribution Review Working Group (CRWG): Issue intake, review coordination
- Legal Agreement Working Group: Legal documentation modeling
- Collateral Working Group: Collateral-specific requirements
- Additional domain-specific groups as needed

**Evidence**:
- Issue #4173: Approved by Legal Agreement Working Group
- Issue #3748: Discussed at CRWG, escalated to Steering WG, currently parked
- Issue #3723: Discussed at TAWG meeting September 11, 2025
- Issue #3432: Approved by Steering WG March 18, 2025
- Meeting minutes documented as GitHub issues (e.g., #2011)

**Process Flow**:
1. New issues marked with "Triage" label
2. Release Management team performs initial triage
3. Routed to appropriate working group
4. Domain working groups review and approve
5. Complex or backward-incompatible changes escalated to Steering WG
6. Approved items added to release milestone

**UML-Codex Implications**:
- Support multi-stakeholder review workflows
- Enable issue routing based on domain area
- Track approval status across multiple groups
- Provide visibility into governance process
- Support escalation paths for complex changes

### 3.2 Triage and Intake Process

**Pattern**: Systematic intake process ensures all issues receive appropriate review before work begins.

**Evidence**:
- "Triage" label: "Requires triage by Release Management team"
- Recent issues marked triage: #4173, #4172, #4169, #4168, #4165, #4139
- FRAGMOS issues in triage: #3739, #3706, #3705
- Prevents premature work on unreviewed proposals
- Ensures alignment with project roadmap

**Benefits**:
- Quality control on incoming requests
- Alignment with strategic direction
- Resource allocation decisions
- Prevent duplicate work
- Maintain model coherence

**UML-Codex Implications**:
- Implement configurable triage workflows
- Support automatic labeling for intake
- Enable assignment to review teams
- Track time in triage state
- Provide triage checklists and criteria

### 3.3 Approval Requirements by Change Type

**Pattern**: Different change types have different approval requirements based on impact.

**Labels and Requirements**:
- **backward-incompatible**: Likely breaking change; must be on Steering WG roadmap; 2 maintainers to approve
- **complex**: Major change; should be broken down into smaller units; Steering WG approval recommended
- **technical**: DSL, mapping, or reference data updates; individual maintainer may approve
- **documentation**: Individual maintainer may approve

**Evidence**:
- 23 open backward-incompatible issues all in Major Release 7.0 milestone
- Complex issues like #3748 require task force formation
- Technical issues like #4032, #4031 have streamlined approval
- Documentation issues like #3936, #3935 progress more quickly

**Benefits**:
- Proportional process overhead to change impact
- Fast track for low-risk changes
- Rigorous review for high-impact changes
- Clear expectations for contributors

**UML-Codex Implications**:
- Support configurable approval workflows by change type
- Automatic classification of change impact
- Required reviewer configuration
- Approval status tracking
- Integration with version control pull request workflows

### 3.4 DSL Governance Challenge

**Challenge**: No defined process for reviewing and accepting changes to Rune DSL, which CDM depends on entirely.

**Evidence**:
- Issue #3723: CDM Needs Defined Process for Rune Changes
- CDM depends entirely on Rune DSL for model definition
- Need to leverage Rune innovation pace while maintaining control
- TAWG should develop governance procedures for SWG approval

**Critical Questions**:
1. Which Rune modifications require CDM proactive evaluation?
2. Should breaking vs non-breaking changes follow distinct approval pathways?
3. Version numbers don't always identify breaking changes - how to detect?
4. What vetting applies to new modeling syntax before CDM adoption?
5. Is TAWG appropriate for all reviews, including urgent changes?
6. How do Rune and CDM manage decisions to reject proposed changes?

**Status**: Discussed at TAWG September 11, 2025; active working group engagement

**Pattern**: Separation between modeling language and domain models requires explicit governance for version management and feature adoption.

**UML-Codex Implications**:
- Separate modeling language from domain models architecturally
- Track DSL version dependencies for each model version
- Implement review process for language feature adoption
- Distinguish breaking vs non-breaking DSL changes
- Support syntax evolution without forcing model updates
- Enable selective adoption of language features
- Document language compatibility matrix

---

## 4. FpML Integration Patterns

### 4.1 Legacy Mapping Maintenance

**Challenge**: Ongoing maintenance burden for FpML synonym mappings with recurring correctness issues.

**Evidence**:
- Issue #4116: Retire or migrate remaining non-FpML legacy mappings (open)
- Issue #4115: Migrate legacy CreateIQ synonym mappings to Ingest functions (open)
- Issue #4076: FpML synonym mapping of PrincipalPaymentSchedule incorrect (open)
- Issue #4039: FpML synonym mapping error for FX instruments (closed)
- Issue #4033: Support for FpML Record-keeping schema (closed)
- Issue #4030: Retire legacy FpML mappings (closed)
- Issue #4031: Contribute FpML Confirmation Ingest functions for CDM 6 (open)

**Evolution Pattern**:
- Original approach: Synonym-based mappings
- Modern approach: Ingest functions
- Migration strategy: Retire legacy, implement new
- Target: Major Release 7.0 for cleanup

**Benefits of Ingest Functions**:
- Better separation of concerns
- Easier testing and validation
- More maintainable codebase
- Clearer transformation logic

**UML-Codex Implications**:
- Support synonym mapping for legacy standard integration
- Implement ingestion function framework for data transformation
- Enable migration path from legacy to modern approaches
- Provide validation tools for mapping correctness
- Support multiple standard versions simultaneously
- Generate mapping code from model definitions
- Test case generation for transformation verification

### 4.2 Multi-Schema Support

**Challenge**: Supporting multiple FpML schema versions and variants (Confirmation, Record-keeping, etc.)

**Evidence**:
- FpML Confirmation Ingest functions (Issue #4031)
- FpML Record-keeping schema support (Issue #4033, closed)
- Different schema versions have different structures
- Need to maintain compatibility across versions
- Dataset validation cleanup required (Issue #4130)

**Pattern**:
- Separate ingest functions per schema variant
- Version-specific transformations
- Validation against CDM model
- Test datasets for each supported version

**UML-Codex Implications**:
- Support schema version management
- Enable variant-specific transformations
- Provide validation frameworks
- Generate test datasets from schemas
- Track compatibility matrix
- Support deprecation of old versions

---

## 5. Securities Lending Domain Extension

**Pattern**: Systematic expansion of CDM for securities lending use cases across multiple dimensions.

**Evidence**:
- Issue #4139: Securities lending visualizations
- Issue #4083: Enhanced security lending qualification
- Issue #4071: Settled position state for lending
- Issue #3896: Eligible collateral enhancements (closed)
- Issue #3871: New fee type options for lending fee and rebate rate
- Issue #3870: Securities transfer type support
- Issue #3697: Relaxed cardinality for TransferableProduct (closed)
- Issue #3640: Rate changes on lending trades
- Issue #3582: Collateral and trade valuations from MTM (closed)
- Issue #3170: Completion lifecycle events implementations
- Issue #3129: Split event needs changeAccount primitive
- Issue #3967: Enhanced GMSLA (Global Master Securities Lending Agreement) support

**Dimensions of Extension**:
1. **Collateral Management**: Eligible collateral specifications and valuation
2. **Fee Structures**: Lending fees, rebate rates, flexible pricing
3. **Transfer Types**: Securities delivery, settlement, return transfers
4. **Rate Mechanisms**: Rate changes during trade lifecycle
5. **Settlement States**: Position states including settled status
6. **Qualification Functions**: Event and trade classification
7. **Lifecycle Events**: Complete event support including allocation
8. **Visualization**: Rosetta DSL visualizations for lending workflows
9. **Legal Agreements**: GMSLA representation and clauses

**Pattern Insight**:
- Domain extensions require coordinated changes across multiple model areas
- Cannot simply add one type - need supporting infrastructure
- Qualification, visualization, lifecycle events, legal agreements all connected
- Phased approach with multiple releases
- Many changes backward-incompatible, targeted to Major Release 7.0

**UML-Codex Implications**:
- Study securities lending as exemplar of domain extension patterns
- Support domain-specific terminology and concepts
- Enable visualization of complex domain workflows
- Support modeling of fee structures and rate mechanisms
- Provide lifecycle event modeling capabilities
- Enable legal agreement and clause modeling
- Support phased rollout of coordinated domain extensions
- Track cross-cutting requirements across model areas

---

## 6. Smart Contract and Blockchain Integration

### 6.1 Smart Contract Framework

**Emerging Use Case**: CDM as foundation for smart contract execution.

**Evidence**:
- Issue #4168: CDM Smart Contract Phase 1 - Collect Floating Rate Option
- Issue #4165: CDM Smart Contract Phase 1 - Smart Contract Framework
- Both marked for triage, high priority
- Phased implementation approach
- Focus on automated execution

**Phase 1 Approach**:
- Collect floating rate options for automated execution
- Develop foundational smart contract framework
- Build on existing CDM structures
- Enable contract automation

**UML-Codex Implications**:
- Consider how domain models map to executable contracts
- Support data collection requirements for execution
- Enable phased implementation of execution capabilities
- Track automation opportunities in domain models
- Study relationship between model and code generation for contracts

### 6.2 Blockchain Identifiers

**Feature Request**: Support for distributed ledger technology identifiers.

**Evidence**:
- Issue #3995: Add DTI and DLI to identify DLT-based financial instruments
- Open since August 2025
- Medium priority
- Complements tokenized assets support

**Context**:
- Issue #3333: Tokenized assets model (closed, approved)
- Modular TokenisationDetails component
- Addresses off-chain/on-chain alignment
- Use cases: trade settlement, collateral management, lifecycle events

**Pattern**:
- Blockchain integration via composition
- Identifier support for DLT instruments
- Maintain compatibility with traditional instruments
- Enable hybrid workflows (off-chain and on-chain)

**UML-Codex Implications**:
- Support blockchain-specific identifier types
- Enable tokenization metadata
- Model off-chain/on-chain relationships
- Support hybrid instrument types
- Track DLT-specific requirements

---

## 7. Rosetta DSL and Code Generation

### 7.1 Multi-Language Code Generation

**Pattern**: Rosetta DSL generates executable code artifacts in multiple programming languages.

**Evidence**:
- Issue #3829: Enum Generator Defect for CDM 7 - Python
- Issue #3878: Use Python Generator for CDM 5.x and 6.x
- Issue #3848: Maven artifact publishing migration
- Issue #3844: Maven artifact publishing for code generators
- Supports: Python, Java, JavaScript
- Published to Maven repositories
- Version-specific generators

**Architecture**:
- Rosetta DSL as source of truth
- Language-specific code generators
- Artifact publishing pipeline
- Distribution mechanism for generated code

**Benefits**:
- Single source of truth in DSL
- Consistent implementations across languages
- Automated code generation reduces errors
- Package management for distribution

**UML-Codex Implications**:
- Study Rosetta code generation architecture
- Implement multi-language code generation
- Support artifact publishing mechanisms
- Enable version-specific generation
- Provide package management integration
- Generate test cases alongside production code

### 7.2 Visualization and Validation

**Capabilities**: Rosetta provides visualization and validation beyond code generation.

**Evidence**:
- Issue #4139: Add securities lending visualizations
- Issue #4130: Dataset validation cleanup and model update
- Visualizations help understand complex workflows
- Validation ensures model correctness and completeness

**Pattern**:
- Visualization generated from model definitions
- Validation rules derived from model constraints
- Dataset validation against model specifications
- Visual debugging of domain logic

**UML-Codex Implications**:
- Support domain model visualization
- Generate validation frameworks from models
- Enable dataset testing against model definitions
- Provide visual debugging capabilities
- Support workflow visualization
- Generate documentation with diagrams

---

## 8. Build and Infrastructure Challenges

### 8.1 CI/CD Reliability

**Challenge**: Build pipeline reliability issues affecting development velocity.

**Evidence**:
- Issue #3925: Regularly broken builds - "Pipeline could not be executed because codefresh.yaml file is missing"
- Issue #4145: Missing PostingObligationsElection class causing build failures (closed)
- Issue #4148: Move build from Codefresh to GitHub Actions (open)
- Build failures particularly during documentation updates
- Master branch build failures

**Impact**:
- Reduced development velocity
- Contributor frustration
- Difficulty identifying real vs spurious failures
- Time wasted debugging infrastructure

**Resolution**:
- Migrate from Codefresh to GitHub Actions
- Better integration with GitHub
- More reliable pipeline execution
- Modern workflow capabilities

**UML-Codex Implications**:
- Integrate with modern CI/CD platforms (GitHub Actions, GitLab CI, etc.)
- Ensure build reliability for code generation outputs
- Provide clear error messages for missing dependencies
- Enable pre-commit validation to catch issues early
- Support local build validation before pushing

### 8.2 Non-Java Build Requirements

**Challenge**: Extending build and distribution beyond Java ecosystem.

**Evidence**:
- Issue #3904: Non-Java CDM build and distribution requirements to support FpML CDM Reference Data Code lists
- Need to support Python, JavaScript, and other languages
- Reference data distribution across languages
- Code list management

**Pattern**:
- Multi-language artifact generation
- Language-specific package management
- Cross-language reference data synchronization
- Unified versioning across artifacts

**UML-Codex Implications**:
- Support language-agnostic build systems
- Enable multiple package managers (npm, pip, Maven, etc.)
- Provide reference data distribution mechanisms
- Maintain version consistency across language artifacts
- Generate language-idiomatic code

---

## 9. Documentation and Community

### 9.1 Documentation Maintenance

**Challenge**: Documentation updates lag behind code, creating barriers to contribution and adoption.

**Evidence**:
- Issue #3572: Contribution guidelines missing (open since March 2025)
- Issue #4128: Documentation link is broken in README
- Issue #3988: CDM Website Updates (open)
- Issue #3936: CDM Documentation - Versioning
- Issue #3935: CDM Documentation - Home.mdx and navigation
- Issue #3934: CDM Documentation - Roadmap
- Issue #3930: CDM Documentation Website Updates following BL/IGZ review
- Multiple closed documentation issues (3933, 3783, 3781, 3780)

**Impact**:
- Barrier to new contributors
- Difficult to understand project structure
- Unclear contribution process
- Out-of-sync API documentation
- Broken links and outdated information

**Pattern**:
- Documentation treated as lower priority than code
- Multiple documentation working items spread over time
- Periodic reviews and updates rather than continuous maintenance
- Community feedback drives improvements (BL/IGZ review)

**UML-Codex Implications**:
- Generate documentation automatically from models
- Keep documentation synchronized with code
- Support documentation versioning with model versions
- Provide contribution workflow automation
- Enable documentation testing (link checking, completeness)
- Support multiple documentation formats (web, PDF, API docs)

### 9.2 Community Engagement

**Pattern**: High community engagement through working group meetings, discussion issues, and collaborative decision-making.

**Evidence**:
- Meeting minutes as GitHub issues (e.g., #2011)
- High comment counts on contentious issues (e.g., #3319 entity identifiers)
- Multiple working groups with regular meetings
- Transparent decision-making processes
- Issue #3432: Debate over property asset integration demonstrates healthy discussion
- FRAGMOS systematic contributions show active community participation

**Benefits**:
- Diverse perspectives improve model quality
- Transparency builds trust
- Collaborative problem-solving
- Knowledge sharing across organizations
- Industry-wide adoption facilitated

**UML-Codex Implications**:
- Support collaborative modeling workflows
- Enable discussion threads on model elements
- Provide meeting management integration
- Track decisions and rationale
- Support multi-organization contribution
- Enable transparent governance processes

---

## 10. Actionable Items for UML-Codex Development

### 10.1 High Priority (Core Capabilities)

1. **Versioning and Backward Compatibility**
   - Implement major/minor release strategy
   - Automatic detection of breaking changes
   - Migration path generation
   - Compatibility analysis tools
   - Milestone-based planning support

2. **State Management and Identity**
   - Distinguish identity attributes from relationships
   - Global key stability analysis
   - Composition vs association guidance
   - Warnings for mutable nested objects affecting parent identity
   - Support for workflow roles separate from entity identity

3. **Multi-Language Code Generation**
   - Study Rosetta DSL architecture
   - Generate Python, Java, JavaScript, TypeScript code
   - Package management integration (Maven, npm, pip)
   - Artifact versioning and publishing
   - Language-idiomatic code generation

4. **Qualification Functions**
   - Track qualification logic alongside models
   - Detect misalignment with model changes
   - Support test case generation
   - Version synchronization
   - Visualization of qualification applicability

5. **Governance Workflows**
   - Multi-stakeholder review processes
   - Configurable approval workflows by change type
   - Triage and intake automation
   - Working group coordination
   - Escalation paths for complex changes

### 10.2 Medium Priority (Enhanced Capabilities)

6. **Component Granularity Analysis**
   - Detect ambiguous multi-purpose components
   - Recommend separation of concerns
   - Track downstream impact of granularity issues
   - Support refactoring tools
   - Complexity metrics and thresholds

7. **Legacy Integration**
   - Synonym mapping framework
   - Ingestion function generation
   - Migration from legacy to modern patterns
   - Validation of transformation correctness
   - Multi-version schema support

8. **Domain Extension Patterns**
   - Modular composition support
   - Core vs external extension analysis
   - Model bloat detection
   - Phased rollout management
   - Cross-cutting change tracking

9. **Systematic Improvement Campaigns**
   - Epic/campaign management
   - Dependency tracking across issues
   - Initiative tagging and filtering
   - Progress visualization
   - Coordinated change planning

10. **Visualization and Validation**
    - Automatic diagram generation from models
    - Workflow visualization
    - Dataset validation frameworks
    - Visual debugging tools
    - Documentation generation with diagrams

### 10.3 Lower Priority (Nice to Have)

11. **DSL Governance**
    - Separate language from domain models
    - DSL version dependency tracking
    - Language feature adoption process
    - Breaking vs non-breaking change detection
    - Compatibility matrix documentation

12. **Build and Infrastructure**
    - CI/CD platform integration (GitHub Actions, GitLab CI)
    - Pre-commit validation hooks
    - Multi-language build support
    - Local build validation
    - Clear error messaging

13. **Documentation Automation**
    - Generate docs from models
    - Version synchronization
    - Link checking and validation
    - Multiple format support
    - API documentation generation

14. **Community Collaboration**
    - Collaborative modeling workflows
    - Discussion threads on model elements
    - Meeting management integration
    - Decision tracking with rationale
    - Multi-organization contribution support

15. **Blockchain and Smart Contracts**
    - DLT identifier support
    - Tokenization metadata
    - Off-chain/on-chain relationship modeling
    - Contract execution framework
    - Hybrid workflow support

---

## 11. Key Lessons for Domain Modeling Tools

### 11.1 Governance is Critical

Domain models used across organizations require:
- Clear approval processes proportional to change impact
- Multi-stakeholder review for complex changes
- Transparent decision-making
- Adequate resourcing for major initiatives
- Escalation paths for contentious issues

### 11.2 Backward Compatibility Requires Planning

Breaking changes should be:
- Clearly labeled and tracked
- Grouped into major releases
- Coordinated with dependent changes
- Documented with migration paths
- Communicated well in advance

### 11.3 Language Separate from Models

DSL evolution should:
- Be independent from domain model evolution
- Have explicit version dependencies
- Support selective feature adoption
- Enable model definition without language coupling
- Provide clear compatibility guarantees

### 11.4 Qualification Logic Needs Attention

Classification and qualification functions:
- Easily become misaligned with model changes
- Require explicit versioning and review
- Need comprehensive test coverage
- Should be visualized alongside models
- Must be updated with each major version

### 11.5 Composition Enables Extension

New capabilities should:
- Use composition over core modification when possible
- Enable modular addition without architectural upheaval
- Maintain flexibility for future enhancement
- Avoid model bloat
- Support domain-specific extensions

### 11.6 Systematic Initiatives Work

Coordinated improvement campaigns:
- Provide holistic view of problem domain
- Enable coherent changes across related areas
- Facilitate dependency management
- Support phased rollout
- Track progress more effectively

### 11.7 Legacy Integration is Hard

Mapping legacy standards:
- Requires ongoing maintenance
- Benefits from modern ingestion functions over synonyms
- Needs validation frameworks
- Should support multiple versions
- Justifies investment in migration tools

### 11.8 Documentation Needs Automation

Documentation should:
- Generate automatically from models
- Stay synchronized with code
- Version with model versions
- Support multiple formats
- Include testing (links, completeness)

### 11.9 Build Reliability Matters

Infrastructure must:
- Integrate with modern CI/CD platforms
- Provide reliable, reproducible builds
- Support multiple languages
- Enable local validation
- Give clear error messages

### 11.10 Community Drives Quality

Open collaboration:
- Brings diverse perspectives
- Improves model quality
- Facilitates adoption
- Shares knowledge across organizations
- Builds trust through transparency

---

## 12. Comparative Analysis with UML-Codex Goals

### 12.1 Alignment

UML-Codex goals align well with CDM patterns:
- **Knowledge aggregation**: CDM demonstrates successful collaborative modeling
- **Regulatory reporting**: CDM is production system for financial regulation
- **Domain modeling**: CDM exemplifies complex financial domain models
- **Workflow integration**: CDM supports lifecycle events and state management
- **Code generation**: Rosetta DSL multi-language generation proven at scale

### 12.2 Gaps to Address

Areas where UML-Codex should learn from CDM:
- **Governance maturity**: Need multi-stakeholder review processes from day one
- **Backward compatibility**: Must plan versioning strategy early
- **Qualification logic**: Should integrate classification functions with models
- **Community tooling**: Need collaboration features for multi-org usage
- **Documentation generation**: Automate to avoid CDM's lag issues

### 12.3 Opportunities

Areas where UML-Codex could improve on CDM:
- **DSL governance**: Build in language/model separation from start
- **Granularity analysis**: Automated detection of overly broad components
- **State management**: Built-in identity vs relationship guidance
- **Migration tools**: Better automated migration for breaking changes
- **Visual debugging**: Enhanced visualization capabilities

---

## 13. Research Questions for Further Investigation

1. **How does Rosetta DSL grammar work internally?**
   - What parsing technology is used?
   - How extensible is the syntax?
   - What validation happens at parse time vs runtime?

2. **How are qualification functions implemented?**
   - Pure functions or stateful?
   - Test framework used?
   - How to ensure exhaustive coverage?

3. **What triggers global key recalculation?**
   - Which attributes contribute to keys?
   - How to prevent unintended key changes?
   - Performance implications?

4. **How does CDM handle schema evolution in practice?**
   - What tools exist for migration?
   - How are adopters notified of changes?
   - What backwards compatibility guarantees exist?

5. **What visualization technology does Rosetta use?**
   - Graphviz? D3.js? Custom?
   - How interactive are visualizations?
   - What diagram types are supported?

6. **How is the working group structure operationalized?**
   - Meeting frequency and format?
   - Decision-making processes?
   - Voting mechanisms?

7. **What testing strategies are used for code generators?**
   - How to test generated code?
   - Round-trip testing?
   - Equivalence testing across languages?

8. **How does Maven publishing work for multiple languages?**
   - How are non-Java artifacts published to Maven?
   - Alternative registries for Python/JavaScript?
   - Version synchronization mechanism?

9. **What motivated the FpML ingestion function approach?**
   - Why move away from synonyms?
   - What problems did it solve?
   - What new capabilities did it enable?

10. **How does CDM handle model composition at runtime?**
    - Dynamic composition or static?
    - Performance characteristics?
    - Validation of composed models?

---

## 14. Conclusion

The FINOS Common Domain Model repository provides invaluable lessons for UML-Codex development:

**Governance and Process**: Mature governance with multi-working-group structure, triage processes, and proportional approval requirements demonstrates how collaborative domain modeling succeeds at scale.

**Version Management**: Coordinated major releases consolidating backward-incompatible changes, parallel minor releases, and clear milestone targeting show effective evolution strategy.

**Technical Patterns**: Composition over core integration, systematic improvement campaigns, qualification function management, and multi-language code generation provide proven architectural patterns.

**Pain Points**: State management fragility, qualification misalignment, insufficient component granularity, and legacy integration burden reveal challenges to anticipate and mitigate.

**Community**: High engagement through working groups, transparent decision-making, and collaborative problem-solving demonstrate the importance of community tooling and processes.

**Rosetta DSL**: Multi-language code generation, visualization, and validation capabilities show the power of DSL-based domain modeling with strong tooling support.

UML-Codex should prioritize: versioning strategy, state management guidance, multi-language code generation, qualification function support, and governance workflows. Learning from CDM's successes and challenges will accelerate UML-Codex development and increase likelihood of adoption.

The FINOS CDM community has created a production-grade financial industry standard through careful governance, technical excellence, and collaborative spirit. UML-Codex should aspire to similar rigor while innovating in areas like DSL governance, granularity analysis, and automated migration tooling.

---

**Next Steps for UML-Codex**:
1. Study Rosetta DSL and code generators in depth
2. Design versioning and backward compatibility strategy
3. Implement state management and identity analysis
4. Create governance workflow framework
5. Build qualification function support
6. Develop multi-language code generation
7. Establish community collaboration features

**Files Referenced**:
- Summary: `/home/user/uml-codex/knowledge-store/issues-analysis/isda-cdm/issues-summary.json`
- Repository: https://github.com/finos/common-domain-model
- Related Analysis: `/home/user/uml-codex/knowledge-store/code-reviews/regnosys/`
