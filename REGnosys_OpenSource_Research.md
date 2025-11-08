# REGnosys Open Source Research

## Summary

REGnosys has open-sourced **some components** of their data platform, but **NOT the core platform itself**. The actual Rosetta data platform backend/server is proprietary/commercial. However, several useful components are available.

## Open Source Repositories

### REGnosys GitHub Organization
**URL:** https://github.com/REGnosys

#### Available Repositories:

1. **rosetta-code-generators**
   - Code generators for various programming languages based on Rosetta DSL
   - Could be useful for generating code from models

2. **rosetta-website**
   - Source code for the Rosetta website
   - May contain UI/UX patterns and documentation structure

3. **rune-dsl**
   - Rune DSL grammar and default code generators
   - Domain-specific language for regulatory reporting
   - Also contributed to FINOS (see below)

4. **cdm-object-builder**
   - Tools for building objects based on Common Domain Model (CDM)
   - Useful for CDM-related work

5. **cdm-starter**
   - Starter project for working with CDM
   - Good reference implementation

6. **public-scripts**
   - Installation scripts for REGnosys software
   - May contain deployment/configuration patterns

## Related Open Source Projects

### FINOS Common Domain Model (CDM)
**URL:** https://github.com/finos/common-domain-model
- Open-source implementation of ISDA CDM in Scala
- REGnosys representatives serve as maintainers
- Standardized model for financial products and lifecycle events

### Rune DSL (FINOS)
- REGnosys contributed Rune DSL to FINOS
- Check FINOS GitHub organization for the repository
- Domain-specific language for regulatory reporting

## What's NOT Open Source

- **Rosetta Platform Core** - The actual data platform backend/server is proprietary
- **Rosetta Platform API** - Backend services are commercial
- **Full Platform Implementation** - Only components/tools are open source

## Key Findings

1. **Limited Open Source**: Only peripheral tools and generators are open source, not the core platform
2. **DSL Available**: The Rune/Rosetta DSL itself is open source, which could be useful
3. **CDM Resources**: Good open source resources around Common Domain Model
4. **Code Generators**: May provide insights into code generation patterns

## Recommendations

1. **Review Code Generators**: The `rosetta-code-generators` repo might have useful patterns for generating code from models
2. **Study DSL**: The Rune DSL implementation could inform your own DSL design
3. **CDM Reference**: Use FINOS CDM as a reference for domain modeling patterns
4. **Website Code**: The website repo might have useful UI/UX patterns

## Next Steps

1. Clone relevant repositories to examine code structure
2. Review code generators for reusable patterns
3. Study DSL implementation for language design insights
4. Check FINOS repositories for additional resources

## Links

- REGnosys GitHub: https://github.com/REGnosys
- REGnosys Website: https://regnosys.com/
- Rosetta Documentation: https://docs.rosetta-technology.io/
- FINOS CDM: https://github.com/finos/common-domain-model

