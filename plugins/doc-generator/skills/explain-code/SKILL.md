---
name: explain-code
description: Explains code in plain language with architecture diagrams. Use when the user asks to understand how code works, needs onboarding help, or wants a high-level explanation of a module or system.
---

When explaining code, follow this structure:

## 1. One-line Summary
Start with a single sentence explaining what this code does at the highest level.

## 2. Architecture Overview
Describe the key components and how they interact. Use a text-based diagram if there are 3+ components:

```
[Component A] --> [Component B] --> [Component C]
       |                                  ^
       +---> [Component D] ---------------+
```

## 3. Key Concepts
List the 3-5 most important concepts someone needs to understand to work with this code. Avoid jargon — explain terms the first time you use them.

## 4. Data Flow
Trace how data moves through the system for the most common operation. Use numbered steps.

## 5. Extension Points
Identify where and how a developer would typically add new functionality.

Adjust depth based on what was asked. If the user asks about a single function, keep it brief. If they ask about a whole module or system, go deeper.
