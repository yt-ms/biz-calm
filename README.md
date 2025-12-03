## Intro

This repo is a proof of concept to explore two aspects of CALM:
- How standard **business** architecture concepts can be expressed in CALM and how they can be related to the technical architecture.
- How different approaches to using JSON schema to extend the core CALM model work, and in particular how the business architecture concepts can be implemented as a CALM **standard**.

Disclaimer: this is at very early stages, and much has been vibed so will need cross-checking and further refinement.


## Quick Start

### Prerequisites

- nodejs >= 24.10.0 (probably works with earlier versions, but not tested).

### Install CALM CLI

```bash
npm install -g @finos/calm-cli
```

### Validate Example Architecture

```bash
cd schemas
calm validate -a ../architectures/trading-system-bizarch.architecture.json
```

> Due to [this issue](https://github.com/finos/architecture-as-code/issues/1863), it is currently necessary to
run the validation from the `schemas` directory as above, rather than from the root or from the `architectures` directory.


## Worked examples of business architecture concepts

* Business Capabilities: `architectures/financial-institution-capabilities.calm.json`
* Value Streams: `architectures/financial-institution-value-streams.calm.json`
* Organizational Structure: `architectures/financial-institution-organization.calm.json`


## Repository Structure

```
biz-calm/
├── architectures/
│   ├── company-example.calm.json
│   ├── financial-institution-capabilities.calm.json
│   ├── financial-institution-organization.calm.json
│   ├── financial-institution-value-streams.calm.json
│   └── trading-system-bizarch.architecture.json
├── schemas/
│   ├── calm-business-architecture.standard.json
│   ├── calm-business-architecture.schema.json
│   ├── calm-extended.meta.json
│   └── company-example.standard.json
├── docs/
│   └── business-architecture-modelling.md
└── README.md
```

## Business architecture concepts supported

The following business architecture core concepts can be used as CALM node types:

- **capability** - Business capabilities
- **value-stream** - Value delivery flows
- **business-process** - Business processes
- **organization-unit** - Organizational structures
- **stakeholder** - Internal/external stakeholders
- **information-concept** - Business information concepts
- **product** - Products and services
- **initiative** - Strategic initiatives
- **strategy** - Strategic plans
- **objective** - Business objectives
- **policy** - Business policies
- **event** - Business events


## Example: 3-Tier Trading System

The example architecture demonstrates:

1. **Business Layer**: Capabilities, value streams, information concepts, stakeholders
2. **Application Layer**: Services implementing business capabilities
3. **Data Layer**: Databases storing business information concepts

### Key Patterns

**Capability Hierarchy**:
```json
{
  "node-type": "capability",
  "metadata": {
    "capability-level": 1,
    "parent-capability": "parent-id"
  }
}
```

**Capability Realization**:
```json
{
  "relationship-type": {
    "composed-of": {
      "container": "business-capability",
      "nodes": ["service", "database"]
    }
  }
}
```

**Value Stream Support**:
```json
{
  "node-type": "service",
  "metadata": {
    "supports-value-stream": "trade-to-settlement",
    "implements-capabilities": ["order-mgmt"]
  }
}
```

## Documentation

- [Business Architecture Modelling Guide](docs/business-architecture-modelling.md) - Comprehensive guide to modeling business architecture concepts
- [Example Architecture](architectures/trading-system-bizarch.architecture.json) - 3-tier trading system
- [Schema Extension](schemas/calm-business-architecture.schema.json) - Business architecture node type definitions

## Validation

CALM accepts custom node types as strings, making business architecture extensions fully schema-compliant:

```bash
# Validate architecture
calm validate -a architectures/trading-system-bizarch.architecture.json

# Generate documentation
calm docify -a architectures/trading-system-bizarch.architecture.json -o docs/output
```

## Use Cases

1. **Enterprise Architecture**: Map business capabilities to application portfolios
2. **Digital Transformation**: Link initiatives to technical modernization
3. **Business Impact Analysis**: Understand technical changes' business impact
4. **Capability-Based Planning**: Organize investments around capabilities
5. **Compliance Mapping**: Trace regulations through capabilities to controls

## Contributing

This is an experimental approach to bridging business and technical architecture. Contributions welcome!

## Resources

- [FINOS CALM](https://calm.finos.org)
- [CALM CLI Documentation](https://github.com/finos/calm)
