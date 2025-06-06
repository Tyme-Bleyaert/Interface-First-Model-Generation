# Interface-First Model Generation (IFMG)

---

## Overview

**Interface-First Model Generation (IFMG)** is a pragmatic software design approach centered around defining data and behavior contracts via interfaces. From these, multiple related model artifacts are automatically generated — including DTOs, business objects, creation/update models, and persistent entities.

It leverages **(incremental) source code generators** to consolidate design intent, eliminate boilerplate, and unify model logic within a single codebase.


## Intend

The intention behind this design is to quickly create crud applications with a central library for the models. There shouldn't be a need to define default mappings or castings between these models as the code generator should create default ones, reducing boilplate code. While still providing enough customization for more advanced use.

---

## Core Principles

### 1. Interface-Centric Design
All models originate from structured interfaces defining shape and behavior. Interfaces serve as the single source of truth for consistency.

### 2. Automated and Centralized Generation
Source generators produce all model types (immutable, mutable, DTOs, entities) from one interface.

### 3. Flexible Property Inclusion/Exclusion
Custom attributes like `[IncludeOnly]` and `[ExcludeFrom]` allow precise property control across model variants.

### 4. Support for Immutable and Mutable Models
Use split interfaces to define immutable (read) and mutable (write) models using native C# constructs.

### 5. Partial Methods and Virtual Properties
Customize via partial methods and virtual properties — enabling clean extensions without modifying generated files.

### 6. Mapping and Casting Integration
Safe, attribute-driven, implicit or explicit operator-based mapping between model types.

---

## Smart Intra-Library Mapping

### Key Enhancement
Automatic mapping logic is generated **within the same project or assembly only**:

- ✅ `Entity ↔ Business Object`
- ✅ `DTO ↔ Command Model`
- ❌ `DTO ↔ Entity` (across libraries) not supported by default

### Why This Matters
- Promotes architectural safety
- Enables seamless automatic mapping
- Ensures generator has full visibility for accurate results

---

## IFMG vs Other Design Approaches

| Design Aspect         | IFMG                           | DDD                          | MDA                          | Contract-First API Design      |
|----------------------|--------------------------------|------------------------------|------------------------------|--------------------------------|
| **Primary Focus**    | Interface-based generation     | Rich domain models           | Meta-model-driven            | API definitions                |
| **Model Org.**       | Centralized                    | Distributed by context       | Tool/spec-driven             | Client/server codegen          |
| **Source of Truth**  | Interfaces                     | Hand-crafted models          | UML/DSL                      | OpenAPI, Protobuf              |
| **Customization**    | Attributes, partials, overrides| Manual                       | Meta-transforms              | Templates/overrides            |
| **Immutability**     | Built-in                       | Optional                     | Tool-dependent               | Target-dependent               |
| **Mapping**          | Auto (intra-lib) + extensible  | Manual or tool-assisted      | Codegen                      | Tool-specific                  |

---

## Why IFMG Is Different

- **Unified Generation** from one interface source
- **Interfaces as central contracts**, not entities
- **Automation-focused** with opt-in control
- **Partial + virtual hooks** for clean extension
- **Incremental and performant** via modern generators

---

## Typical IFMG Workflow

1. **Define Interfaces**  
2. **Annotate Properties** (with include/exclude attributes)  
3. **Run Generator** (during build)  
4. **Customize via Hooks** (partials/overrides)  
5. **Consume Models** (in APIs, services, persistence)

---

## Benefits Recap

- ✅ **Consistency** via shared contracts
- ⚡ **Productivity** from generated boilerplate
- 🧩 **Flexibility** through attributes and hooks
- 📈 **Scalability** via incremental generation
- 🚧 **Separation of concerns** built-in

---

## When to Use IFMG

- Projects with many model types (DTOs, BOs, Entities)
- Teams favoring automation and contract enforcement
- Systems valuing immutability and clear model boundaries

---

## DDD Compatibility in IFMG

| DDD Concept           | IFMG Support     | Notes                                                  |
|----------------------|------------------|--------------------------------------------------------|
| **Entities**         | ✅ Generated      | Extendable for aggregates                              |
| **Value Objects**    | ✅ Read-only      | Supports immutability + equality overrides             |
| **Aggregates**       | 🔶 Manual         | Wrap or compose generated types                        |
| **Domain Services**  | ✅ Compatible     | Can build around IFMG interfaces                       |
| **Repositories**     | ✅ Compatible     | Prefer logic separated from infrastructure             |
| **Layered Arch.**    | 🔶 Optional       | Use separate generators/libraries                      |
| **Bounded Contexts** | 🔶 Manual         | Modular interfaces simulate context boundaries         |

### Limitations & Mitigations

- **Layer Separation** → Use bridge libraries or separate generator projects  
- **Behavior-rich Models** → Extend with wrappers or partials  
- **Bounded Contexts** → Organize interfaces by domain module  

---

## Pattern Compatibility with IFMG

| Pattern                    | Compatibility                             |
|---------------------------|-------------------------------------------|
| 🧱 Clean Architecture      | ✅ Interfaces in core, models per layer     |
| 🔄 CQRS                  | ✅ Split read/write interfaces              |
| 🏗️ Builder Pattern         | ✅ Build using mutable creation models      |
| 📬 Mediator (MediatR)      | ✅ Models can implement `IRequest<T>`       |
| 🔌 Adapter / ACL           | ✅ Use DTOs to isolate external contracts   |
| 🎨 Decorator Pattern       | ✅ Use virtuals/partials to extend          |
| 🔎 Specification Pattern   | ✅ Use for query expressions                |
| 🧪 Factory Pattern         | ✅ Feed generated models into factories     |
| 🎯 Strategy Pattern        | ✅ Customize with partials, overrides       |
| 📣 Eventing / Observer     | ✅ Trigger partial lifecycle hooks          |

---

## Where IFMG May Not Be Ideal

- Systems with deeply **behavior-embedded models**
- Projects relying on **runtime type mutation**
- Heavy **AOP/runtime weaving**
- Environments needing **dynamic model composition**
- GraphQL or scripting systems needing fine-grained, runtime-determined models

---

## Summary

IFMG provides a **clean, contract-first, automation-driven** foundation for model management. It aligns with modern practices and architectural patterns while staying lightweight and developer-friendly. For new projects, especially in C#, IFMG brings clarity, scalability, and maintainability right out of the gate.
