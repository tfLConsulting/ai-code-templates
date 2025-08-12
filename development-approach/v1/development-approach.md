# Iterative Specification-First Development Approach v1.0

## Philosophy

This document describes an **evolved iterative specification-first development approach** optimized for AI-assisted development. We develop through rapid cycles of focused features while maintaining clear separation between system capabilities and development history.

## Core Principles

1. **Hypothesis-Driven Development**: Each feature starts with a hypothesis about user needs
2. **Focused Feature Decomposition**: Break complex features into standalone capabilities
3. **AI-Optimized Context**: Specifications sized for complete AI understanding (\~1500 words max)
4. **Rapid Prototyping**: Build minimal implementations to validate understanding
5. **Continuous Learning**: Capture learnings in development history, not feature specs
6. **Separation of Concerns**: Distinguish between "what the system does" and "how we built it"

## Development Cycles

### Enhanced Feature Development Cycle

```
Hypothesis → Focused Spec → Implementation → Learning → Quality → Next Feature
     ↑                                                     ↓         ↓
     ←←←←←←←←←←← Refactoring Cycle (every 3-5 features) ←←←←←
```

### Primary Cycle (1-3 days per feature)

1. **Hypothesis Formation**

   * What specific user capability are we enabling?
   * What's our theory about the technical approach?
   * What assumptions need validation?

2. **Focused Feature Specification**

   * Document capability, interface, dependencies
   * Define success criteria and test cases
   * Keep AI-assistant friendly (focused, complete context)

3. **Minimal Implementation**

   * Build simplest version that proves the concept
   * Focus on core functionality, defer edge cases
   * Use AI assistants for rapid development

4. **Learning Documentation**

   * Record implementation approach and results in development-history
   * Document what worked, what didn't, and why
   * Update feature specs if interface changes

### Secondary Cycle (0.5-1 day every 3-5 features)

5. **Quality & Refactoring**

   * Code organization and structure improvements
   * Test coverage enhancement
   * Documentation synchronization
   * Technical debt reduction

### Tertiary Cycle (0.5 day every 8-10 features)

6. **Architecture Review**

   * Cross-cutting concerns evaluation
   * System integration assessment
   * Performance and scalability review

## Dual Documentation Structure

### Separation of Concerns

We maintain two distinct types of documentation:

**Feature Specifications** → What the system does (permanent, evolving)
**Development History** → How we built it (append-only, chronological)

### Example Directory Structure

```
specs/
├── development-approach.md              # This document
├── features/                            # WHAT the system does
│   ├── schema-validation/               # Focused capabilities
│   │   ├── capability.md                # What this feature enables
│   │   ├── interface.md                 # Inputs, outputs, errors
│   │   ├── dependencies.md              # What it needs from other features
│   │   └── tests.md                     # Success criteria and test cases
│   ├── ai-structure-generation/
│   ├── preview-rendering/
│   ├── input-interface-design/
│   └── api-client-communication/
├── development-history/                 # HOW we built it
│   ├── schema-implementation/           # Implementation chronicles
│   │   ├── plan.md                      # Detailed task breakdown and progress tracking
│   │   ├── hypothesis.md                # Original theory
│   │   ├── approach.md                  # Technical approach taken
│   │   ├── results.md                   # What was delivered
│   │   └── learnings.md                 # Insights and discoveries
│   ├── ai-integration-implementation/
│   ├── frontend-preview-implementation/
│   └── refactoring-cycle-1/
├── patterns/                            # Reusable approaches
│   ├── component-patterns.md
│   ├── testing-patterns.md
│   └── ai-integration-patterns.md
└── architecture/                        # System-level design
    ├── overview.md
    ├── data-flow.md
    └── technology-choices.md
```

## Feature Specification Templates

### Feature Capability Document

```markdown
# Feature: [Name]

## User Capability
What specific ability does this give users?

## System Behavior
How does the system behave when this feature is used?

## Success Metrics
- Performance criteria (response time, success rate)
- Quality criteria (accuracy, reliability)
- User experience criteria (usability, satisfaction)

## Status
- ✅ Implemented | 🔄 In Progress | ⏳ Planned | ❌ Deprecated
```

### Feature Interface Document

```markdown
# Feature Interface: [Name]

## Public API
- **Inputs**: Request types, parameters, validation rules
- **Outputs**: Response types, data structures
- **Errors**: Error conditions and handling

## Integration Points
- Frontend components that use this feature
- Backend services that provide this feature
- External APIs or services involved

## Examples
Concrete usage examples with sample data
```

### Feature Dependencies Document

```markdown
# Feature Dependencies: [Name]

## Required Features
Features that must exist for this to work:
- feature-name: What aspect is needed

## Optional Features
Features that enhance this one if available:
- feature-name: How it enhances functionality

## Provides To
Features that depend on this one:
- feature-name: What they use from this feature

## External Dependencies
Third-party services, APIs, libraries required
```

## Development History Templates

### Implementation Chronicle

```markdown
# Plan: [Task Name]

## Task Breakdown

### Phase 1: [Name]
- [ ] Specific subtask 1
- [ ] Specific subtask 2
- [ ] Specific subtask 3

### Phase 2: [Name]
- [ ] Specific subtask 1
- [ ] Specific subtask 2

## Completion Criteria
- [ ] Primary objective achieved
- [ ] Success metrics met
- [ ] Quality gates passed
- [ ] Documentation updated

## Dependencies
- External dependencies that must be resolved
- Prerequisites from other features
- Validation requirements

## Risk Mitigation
- Identified risks and mitigation strategies
- Rollback plans for major changes
- Testing approach for validation
```

```markdown
# Implementation: [Task Name]

## Hypothesis
What we believed about the problem and solution

## Approach Taken
- Technical strategy chosen
- Key implementation decisions
- Tools and libraries used

## Results Delivered
- What was actually built
- Performance metrics achieved
- Functionality delivered

## Learnings
- ✅ What worked well
- ❌ What didn't work as expected
- 💡 Unexpected discoveries
- 🔄 What we'd do differently

## Impact on Future Work
How these learnings affect subsequent features

## Artifacts
- Code locations
- Test files
- Documentation created
- Tools or scripts built
```

### Plan-Driven Implementation Workflow

**The plan.md file serves as the implementation roadmap and progress tracker:**

1. **Pre-Implementation**: Create plan.md with detailed task breakdown based on hypothesis and approach
2. **During Implementation**: Check off tasks as completed, update with blockers or scope changes
3. **Post-Implementation**: Final checklist completion becomes input for results.md

**Benefits of plan-driven workflow:**

* **Progress visibility**: Clear task breakdown shows implementation progress
* **Scope management**: Planned tasks help identify scope creep or missing requirements
* **Context preservation**: Detailed task list captures implementation decisions
* **Parallel work enablement**: Clear task boundaries enable multiple developers

**Plan.md integration with other files:**

* **Hypothesis → Plan**: Theoretical approach becomes concrete task breakdown
* **Plan → Results**: Completed plan tasks become achieved deliverables
* **Plan updates → Learnings**: Changes to original plan become insights about complexity

## Task Management Integration

### User Stories vs Features vs Tasks vs History

**User Stories** (permanent, user-focused):

* Complete user journey scenarios from start to finish
* Success criteria defined from user perspective
* Context about user goals, motivations, and constraints
* Cross-cutting concerns that span multiple features

**Feature Specifications** (permanent, system-focused):

* System capabilities and interfaces
* Technical implementation details
* Integration and dependency information
* Component-level functionality

**Development Tasks** (temporary, implementation-focused):

* Specific implementation work items
* Bug fixes and improvements
* Testing and validation tasks
* Refactoring and cleanup work

**Development History** (permanent, append-only record):

* Implementation approaches and decisions
* Results and learnings from development work
* Technical insights and discoveries

### Enhanced Workflow with User Stories

1. **User Story Definition**: Identify complete user scenarios and success journeys
2. **Feature Planning**: Create feature specs based on user story requirements
3. **Task Creation**: Break features into specific implementation tasks
4. **Development**: Execute tasks while documenting approach
5. **User Story Validation**: Test complete user journeys against story criteria
6. **History Recording**: Document results and learnings
7. **Feature Updates**: Update specs if interface or behavior changes

### Story-Driven Development Process

**Story Phase**:

* Define user personas and their specific goals
* Map complete user journeys from start to finish
* Identify success criteria and pain points to avoid
* Document conversation flows and interaction patterns

**Feature Phase**:

* Translate user stories into system capabilities
* Define technical interfaces and dependencies
* Plan component-level implementations
* Ensure features serve story requirements

**Task Phase**:

* Break features into 1-3 day implementation cycles
* Maintain focus on user story outcomes
* Test against story criteria throughout development
* Document learnings for future story implementations

## AI-Assistant Optimization

### Context Management Strategy

**Focused Context Windows**:

* Single feature complete in AI context (\~1500 words)
* Clear dependencies and interfaces
* Concrete examples and patterns

**Parallel Work Enablement**:

* Features designed for independent development
* Clear interfaces enable parallel AI sessions
* Minimal cross-dependencies

**Learning Integration**:

* Development history provides patterns for AI assistants
* Successful approaches documented for reuse
* Anti-patterns captured to avoid repetition

### AI-Friendly Request Patterns

**User Story Development Request**:

```
"Implement user story [story-name]:
- Reference: specs/user-stories/[story-name].md
- User goal: [specific user outcome]
- Success criteria: [from story definition]
- Features involved: [list of system capabilities needed]
- Development approach: [any constraints or preferences]"
```

**Feature Development Request**:

```
"Implement [feature-name] capability:
- Reference: specs/features/[feature-name]/
- Supporting user stories: [list relevant stories]
- Dependencies: [list required features]
- Success criteria: [from tests.md]
- Development approach: [any constraints or preferences]"
```

**User Story Validation Request**:

```
"Validate user story [story-name] completion:
- Test complete user journey from story definition
- Verify success criteria are met
- Validate cross-device experience
- Document any story refinements needed"
```

**History Documentation Request**:

```
"Document implementation of [task-name]:
- Create development-history/[task-name]/
- Include: hypothesis, approach, results, learnings
- Reference: code changes made during implementation
- Note impact on user stories: [relevant story outcomes]"
```

## Benefits of This Approach

### For Development Process

* **Risk Management**: Focused features isolate complexity
* **Quality Control**: Explicit refactoring cycles prevent debt accumulation
* **Knowledge Retention**: Development history preserves decision context
* **Team Coordination**: Clear interfaces enable parallel work

### For AI-Assisted Development

* **Optimal Context**: Specifications fit AI context windows perfectly
* **Pattern Reuse**: History provides templates for similar implementations
* **Error Recovery**: Clear separation helps debug issues efficiently
* **Rapid Iteration**: Focused features enable quick hypothesis testing

### For Long-term Maintenance

* **System Understanding**: Feature specs document current capabilities
* **Decision Context**: History explains why technical choices were made
* **Evolution Support**: Clear dependencies guide system changes
* **Onboarding**: New developers can understand both what and why

## Getting Started with New Approach

### For Next Feature

1. **Create feature specification** in `specs/features/[name]/`
2. **Create implementation tasks** using todo system
3. **Document approach** as you implement
4. **Record learnings** in `specs/development-history/[task-name]/`
5. **Update feature specs** if interface changes during implementation

### Migration Strategy

* **Continue current work** with existing structure
* **Apply new approach** to next feature
* **Gradually migrate** existing specs during natural updates
* **Preserve learnings** from current sub-component approach

This approach maintains the strengths of specification-first development while addressing complexity and maintenance challenges discovered during prior implementations.
