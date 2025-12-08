# PSP - Prompt Suggestion Protocol Specification

**Status:** Draft  
**Version:** 0.1.0  
**Last Updated:** 2025-12-08

## Abstract

The Prompt Suggestion Protocol (PSP) defines a standardized tagging architecture for Large Language Models (LLMs). This specification establishes a common framework for annotating, categorizing, and managing prompts across different LLM platforms and implementations.

## Table of Contents

1. [Introduction](#introduction)
2. [Terminology](#terminology)
3. [Architecture Overview](#architecture-overview)
4. [Tag Structure](#tag-structure)
5. [Core Tag Types](#core-tag-types)
6. [Implementation Requirements](#implementation-requirements)
7. [Security Considerations](#security-considerations)
8. [Extensibility](#extensibility)
9. [References](#references)

## 1. Introduction

### 1.1 Purpose

This specification defines the Prompt Suggestion Protocol (PSP), a tagging architecture designed to provide a standardized method for organizing, discovering, and managing prompts used with Large Language Models.

### 1.2 Scope

This document describes:
- The structure and syntax of PSP tags
- Core tag types and their semantics
- Requirements for PSP-compliant implementations
- Guidelines for extending the protocol

### 1.3 Design Goals

The PSP tagging architecture aims to:
- Enable consistent prompt categorization across platforms
- Facilitate prompt discovery and reuse
- Support hierarchical organization of prompts
- Allow for domain-specific extensions
- Maintain compatibility with existing LLM workflows

## 2. Terminology

**Tag**: A metadata annotation applied to a prompt or prompt template.

**Prompt**: A text input provided to a Large Language Model to elicit a specific type of response.

**Prompt Template**: A reusable prompt structure with variable placeholders.

**Tag Namespace**: A logical grouping of related tags.

**Tag Hierarchy**: A parent-child relationship between tags enabling structured categorization.

## 3. Architecture Overview

### 3.1 Core Concepts

The PSP architecture is built on the following principles:

1. **Hierarchical Tagging**: Tags can be organized in parent-child relationships
2. **Namespace Isolation**: Tags are grouped into namespaces to prevent conflicts
3. **Extensibility**: Custom tags and namespaces can be defined for specific use cases
4. **Interoperability**: Standard tag formats ensure cross-platform compatibility

### 3.2 System Components

A PSP-compliant system consists of:
- **Tag Registry**: A repository of defined tags and their metadata
- **Tag Validator**: A component that ensures tags conform to the specification
- **Tag Resolver**: A mechanism for resolving tag relationships and hierarchies

## 4. Tag Structure

### 4.1 Tag Syntax

Tags follow this general syntax:

```
namespace:category[.subcategory]
```

Where:
- `namespace` identifies the tag namespace (required)
- `category` specifies the primary classification (required)
- `subcategory` provides additional classification granularity (optional)

### 4.2 Tag Naming Conventions

- Tag names MUST use lowercase alphanumeric characters and hyphens
- Namespaces MUST be separated from categories by a colon (`:`)
- Subcategories MUST be separated by periods (`.`)
- Tag names SHOULD be descriptive and concise

**Examples:**
```
psp:domain.code-generation
psp:format.structured-output
psp:task.summarization
```

### 4.3 Reserved Namespaces

The following namespaces are reserved:
- `psp`: Core PSP tags defined in this specification
- `psp-ext`: Official extensions to the PSP standard
- `x-*`: Experimental or vendor-specific tags

## 5. Core Tag Types

### 5.1 Domain Tags

Domain tags categorize prompts by subject area or field of application.

**Namespace:** `psp:domain`

**Examples:**
- `psp:domain.code-generation`
- `psp:domain.creative-writing`
- `psp:domain.data-analysis`
- `psp:domain.education`

### 5.2 Task Tags

Task tags describe the primary objective or function of a prompt.

**Namespace:** `psp:task`

**Examples:**
- `psp:task.summarization`
- `psp:task.translation`
- `psp:task.classification`
- `psp:task.question-answering`

### 5.3 Format Tags

Format tags specify the expected structure or format of the LLM response.

**Namespace:** `psp:format`

**Examples:**
- `psp:format.json`
- `psp:format.markdown`
- `psp:format.structured-output`
- `psp:format.free-text`

### 5.4 Complexity Tags

Complexity tags indicate the sophistication level required for effective prompt use.

**Namespace:** `psp:complexity`

**Examples:**
- `psp:complexity.basic`
- `psp:complexity.intermediate`
- `psp:complexity.advanced`
- `psp:complexity.expert`

## 6. Implementation Requirements

### 6.1 Minimum Compliance

A PSP-compliant implementation MUST:
1. Support the core tag namespaces defined in Section 5
2. Validate tag syntax according to Section 4.1
3. Preserve tag metadata when storing or transmitting prompts
4. Provide a mechanism for querying prompts by tags

### 6.2 Tag Storage

Implementations SHOULD store tags in a structured format that:
- Preserves tag hierarchy
- Supports efficient querying
- Allows for tag versioning
- Maintains tag relationships

### 6.3 Tag Discovery

Implementations SHOULD provide mechanisms for:
- Listing available tags
- Searching tags by namespace or category
- Resolving tag hierarchies
- Suggesting relevant tags based on prompt content

## 7. Security Considerations

### 7.1 Tag Injection

Implementations MUST sanitize tag input to prevent:
- Malicious tag injection
- Cross-namespace tag conflicts
- Tag spoofing

### 7.2 Access Control

Implementations SHOULD provide:
- Mechanisms for controlling tag creation and modification
- Namespace-level access controls
- Audit logging for tag operations

## 8. Extensibility

### 8.1 Custom Namespaces

Organizations may define custom namespaces for domain-specific tags:

```
org-name:category[.subcategory]
```

Custom namespaces MUST NOT conflict with reserved namespaces.

### 8.2 Tag Extension

New tag categories can be proposed through:
1. Community discussion and feedback
2. Formal RFC process for `psp-ext` namespace additions
3. Vendor-specific extensions using `x-vendor` namespace

## 9. References

### 9.1 Normative References

- [RFC 2119](https://tools.ietf.org/html/rfc2119) - Key words for use in RFCs to Indicate Requirement Levels

### 9.2 Informative References

- Large Language Model documentation and best practices
- Prompt engineering guidelines
- Metadata and tagging standards

---

## Appendix A: Example Implementations

### A.1 JSON Representation

```json
{
  "prompt": "Analyze the following data and provide insights...",
  "tags": [
    "psp:domain.data-analysis",
    "psp:task.analysis",
    "psp:format.structured-output",
    "psp:complexity.intermediate"
  ],
  "metadata": {
    "version": "1.0",
    "author": "example-user",
    "created": "2025-12-08T00:00:00Z"
  }
}
```

### A.2 YAML Representation

```yaml
prompt: "Analyze the following data and provide insights..."
tags:
  - psp:domain.data-analysis
  - psp:task.analysis
  - psp:format.structured-output
  - psp:complexity.intermediate
metadata:
  version: "1.0"
  author: "example-user"
  created: "2025-12-08T00:00:00Z"
```

## Appendix B: Change Log

### Version 0.1.0 (2025-12-08)
- Initial draft specification
- Core tag types defined
- Basic architecture outlined
