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
* Value Stream Stages & Process Mapping: `architectures/financial-institution-loan-process.calm.json`
* Organizational Structure: `architectures/financial-institution-organization.calm.json`


## Repository Structure

```
biz-calm/
├── architectures/
│   ├── company-example.calm.json
│   ├── financial-institution-capabilities.calm.json
│   ├── financial-institution-loan-process.calm.json
│   ├── financial-institution-organization.calm.json
│   ├── financial-institution-value-streams.calm.json
│   └── trading-system-bizarch.architecture.json
├── schemas/
│   ├── calm-business-architecture.standard.json
│   ├── calm-business-architecture.schema.json
│   ├── calm-extended.meta.json
│   └── company-example.standard.json
└── README.md
```

## Business architecture concepts supported

The following business architecture core concepts can be used as CALM node types:

- **business-capability** - Business capabilities with hierarchical decomposition
- **value-stream** - Value delivery flows from stakeholder perspective
- **value-stream-stage** - Sequential stages within a value stream
- **business-process** - Business processes that implement capabilities and stages
- **organization-unit** - Organizational structures and units
- **stakeholder** - Internal/external stakeholders
- **information-concept** - Business information concepts
- **product** - Products and services
- **initiative** - Strategic initiatives
- **strategy** - Strategic plans
- **objective** - Business objectives
- **policy** - Business policies
- **event** - Business events


### Examples


**Business Capability**:
```json
{
  "unique-id": "credit-risk-management",
  "node-type": "business-capability",
  "name": "Credit Risk Management",
  "description": "Ability to evaluate and manage the risk of loss from borrower default...",
  "capability-level": 1,
  "parent-capability": "risk-management",
  "seq-num": "1.1"
}
```

**Capability Hierarchy Relationship**:
```json
{
  "unique-id": "rel-credit-risk-to-risk",
  "relationship-type": "hierarchy",
  "parties": ["risk-management", "credit-risk-management"],
  "description": "Credit Risk Management is a sub-capability of Risk Management"
}
```

**Value Stream**:
```json
{
  "unique-id": "process-loan-application",
  "node-type": "value-stream",
  "name": "Process Loan Application",
  "description": "Complete journey from loan inquiry through funding...",
  "stakeholder": "retail-customer",
  "value": "Approved loan with funds disbursed to meet customer's financial need"
}
```

**Value Stream Stage**:
```json
{
  "unique-id": "vs-stage-assess-credit",
  "node-type": "value-stream-stage",
  "name": "Assess Credit Risk",
  "description": "Comprehensive evaluation of applicant's creditworthiness...",
  "parent-value-stream": "process-loan-application",
  "stage-order": 2,
  "stakeholder": "loan-officer",
  "stage-value": "Completed credit risk assessment with recommendation score"
}
```

**Value Stream Composition**:
```json
{
  "relationship-type": {
    "composed-of": {
      "container": "process-loan-application",
      "nodes": ["vs-stage-receive-application", "vs-stage-assess-credit"]
    }
  },
  "description": "Process Loan Application value stream is composed of stages"
}
```

**Stage-to-Process Mapping**:
```json
{
  "relationship-type": {
    "connects": {
      "source": {"node": "vs-stage-assess-credit"},
      "destination": {"node": "proc-score-application"}
    }
  },
  "metadata": {
    "relationship-category": "maps-to",
    "description": "Value stream stage to process mapping"
  }
}
```

**Stakeholder Interaction**:
```json
{
  "relationship-type": {
    "interacts": {
      "actor": "retail-customer",
      "nodes": ["process-loan-application"]
    }
  },
  "description": "Retail Customer initiates Process Loan Application value stream"
}
```

## Further examples

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
5. **Value Stream Mapping**: Decompose value streams into stages and map to processes
6. **Process-to-Capability Alignment**: Connect operational processes to strategic capabilities
7. **Compliance Mapping**: Trace regulations through capabilities to controls

## Contributing

This is an experimental approach to bridging business and technical architecture. Contributions welcome!

## Resources

- [FINOS CALM](https://calm.finos.org)
- [CALM CLI Documentation](https://github.com/finos/calm)
