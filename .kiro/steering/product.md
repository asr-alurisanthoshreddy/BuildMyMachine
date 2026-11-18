# BuildMyMachine — Product Definition

## 1. Product Identity

**Product Name:** BuildMyMachine

**Product Vision:** Explore how machines are built. Understand how they work.

BuildMyMachine is an interactive 3D engineering learning platform that enables users to explore, understand, assemble, and simulate different types of machines.

The platform aims to bridge the gap between theoretical engineering knowledge and practical understanding by providing an interactive environment where users can examine machine structures, understand individual components, assemble mechanical systems, and observe how machines operate.

BuildMyMachine is designed as a cross-domain platform covering industrial machinery, automobiles, robotics, and other mechanical and engineering systems.

It is not limited to a single machine category or engineering discipline.

## 2. Problem Statement

Understanding how machines are constructed and how their components work together can be challenging through traditional learning methods alone.

Some of the key problems are:

- Engineering education frequently relies on textbooks, diagrams, static images, and videos to explain mechanical systems.
- Students may understand individual components but struggle to understand how those components interact within a complete machine.
- Access to physical machines, workshops, and practical assembly experiences can be limited.
- Understanding mechanical relationships, component positioning, and machine operation can be difficult without interactive visualization.
- Learners need opportunities to experiment with different configurations and observe machine behaviour.

BuildMyMachine addresses these challenges through interactive 3D exploration, guided learning, free assembly, and machine simulation.

## 3. Product Objectives

The primary objectives of BuildMyMachine are:

1. Make complex machines easier to understand through interactive 3D visualization.
2. Help users understand the purpose and functionality of individual machine components.
3. Provide guided experiences for learning how machines are assembled.
4. Allow users to independently experiment with components and build custom configurations.
5. Demonstrate how assembled machines operate through interactive simulation.
6. Create a reusable platform that can expand across multiple engineering domains.
7. Connect theoretical engineering concepts with practical, visual learning experiences.

## 4. Target Audience

BuildMyMachine is intended for:

- **Engineering Students:** Students who want to understand mechanical systems beyond theoretical explanations.
- **Engineering Learners:** Individuals interested in learning how machines and their components work.
- **Mechanical and Automotive Enthusiasts:** People interested in machine structures, mechanisms, and automotive systems.
- **Robotics Learners:** Individuals interested in robotic mechanisms, actuators, and automation.
- **Educators and Trainers:** People who want interactive visual resources for teaching engineering concepts.
- **Technical Professionals:** Engineers and technical professionals who want to explore mechanical concepts and configurations.

The platform should accommodate beginners while providing meaningful technical depth for more experienced users.

## 5. Product Domains

BuildMyMachine must support multiple machine categories through an extensible product structure.

### 5.1 Industrial Machinery

Examples:

- Automatic bottle-capping machines.
- Conveyor systems.
- CNC machines.
- Robotic manufacturing systems.
- Industrial automation equipment.
- Production and packaging machinery.

The initial machine experience will focus on an automatic bottle-capping machine.

### 5.2 Automobiles and Automotive Systems

Examples:

- Internal combustion engines.
- Electric vehicle powertrains.
- Transmission systems.
- Braking systems.
- Suspension systems.
- Steering mechanisms.

### 5.3 Robotics

Examples:

- Industrial robotic arms.
- Robotic manipulators.
- Humanoid robot mechanisms.
- Autonomous robotic systems.
- Actuators and motion mechanisms.

### 5.4 Mechanical and Engineering Systems

Examples:

- Pumps.
- Turbines.
- Generators.
- Gearboxes.
- Hydraulic systems.
- Pneumatic systems.
- Other mechanical assemblies.

### 5.5 Future Domains

The platform should be capable of expanding into additional machine-based engineering domains without requiring the entire product to be redesigned.

These categories represent the intended product direction, not a requirement to implement every domain in the initial release.

## 6. Core Product Capabilities

### 6.1 Interactive 3D Machine Exploration

Users should be able to explore machines in an interactive 3D environment.

Expected capabilities:

- View complete machines in 3D.
- Rotate, zoom, and navigate around machines.
- Inspect individual components.
- Select components to understand their purpose.
- View component names, descriptions, and relevant information.
- Explore relationships between individual components and the complete machine.
- Examine internal structures where supported.

The objective is to help users understand both the overall machine structure and its individual parts.

### 6.2 Guided Assembly Mode

Guided Assembly provides a structured, step-by-step experience for learning how a machine is constructed.

Expected capabilities:

- Present predefined assembly instructions.
- Guide users through individual assembly steps.
- Highlight the component required for the current step.
- Highlight the appropriate mounting or connection locations.
- Explain the purpose of each component.
- Provide hints and guidance when needed.
- Validate whether the expected assembly operation has been completed.
- Track assembly progress.
- Allow users to understand why components are positioned and connected in a particular way.

Guided Assembly should focus on learning, understanding, and correctly completing a predefined machine assembly.

### 6.3 Free Assembly Mode

Free Assembly provides an open environment where users can experiment with machine components and create their own configurations.

Expected capabilities:

- Browse and access reusable machine components.
- Select components from the component library.
- Add components to the 3D workspace.
- Move, rotate, and position components.
- Connect compatible components.
- Support appropriate connection and placement constraints.
- Experiment with different machine configurations.
- Create custom assemblies without following predefined instructions.

Free Assembly should encourage experimentation and independent learning.

### 6.4 Shared Assembly System

Guided Assembly and Free Assembly must use the same underlying component ecosystem and assembly capabilities.

The two modes should differ in their user experience and interaction rules, not in the fundamental representation of machine components.

The shared system should support:

- Reusable components.
- Component placement and positioning.
- Mechanical connections.
- Compatibility validation.
- Assembly hierarchy.
- Component relationships.
- Consistent component behaviour across machine experiences.

Avoid creating separate, duplicated component systems for each assembly mode.

### 6.5 Machine Simulation

Machine Simulation enables users to observe how machines operate after assembly or configuration.

Expected capabilities:

- Simulate relevant mechanical movements.
- Demonstrate the operation of motors and actuators.
- Represent sensors and their interactions.
- Visualize machine operating sequences.
- Demonstrate interactions between connected components.
- Allow users to start, pause, resume, and reset supported simulations.
- Help users understand how individual mechanisms contribute to overall machine operation.

Simulation behaviour must be appropriate to the supported machine and its intended learning objectives.

Visual animation alone must not be presented as proof of physically accurate simulation.

### 6.6 Interactive Learning and Component Understanding

Learning should be integrated throughout the platform rather than restricted to separate textual lessons.

Users should be able to:

- Understand what individual components do.
- Learn how components function.
- Explore how multiple components work together.
- Understand the operating principles of complete machines.
- Learn through assembly and experimentation.
- Receive explanations relevant to the machine or component being explored.

The platform should make engineering concepts understandable through interaction and visualization.

## 7. Reusable Component Ecosystem

A reusable component ecosystem is a fundamental part of BuildMyMachine.

The platform should maintain a library of components that can be reused across different machines and engineering domains.

Potential components include:

- Electric motors.
- Gearboxes.
- Gears.
- Shafts.
- Bearings.
- Conveyors.
- Frames and structural supports.
- Pneumatic cylinders.
- Hydraulic actuators.
- Sensors.
- Grippers.
- Robotic joints.
- Mechanical fasteners.
- Controllers and other relevant machine components.

Each component should have an identifiable function, relevant properties, and appropriate connection capabilities.

Components should not be unnecessarily duplicated for individual machine experiences.

The component ecosystem should support future expansion into additional engineering domains.

## 8. Primary User Experience

The general user journey should follow this progression:

1. **Discover:** Browse available machine categories and machine experiences.
2. **Explore:** Open a machine and inspect its 3D structure.
3. **Understand:** Select components and learn their purposes and relationships.
4. **Assemble:** Choose Guided Assembly or Free Assembly.
5. **Experiment:** Position and connect components where supported.
6. **Simulate:** Observe how the assembled machine operates.
7. **Learn:** Understand the mechanical principles and interactions involved.

Not every machine experience must support every capability immediately. The available functionality should be clearly communicated to users.

## 9. Product Design Principles

BuildMyMachine should follow these product-level principles:

### 9.1 Interaction Over Static Presentation

The platform should prioritize interactive exploration over simply displaying machine images or explanatory text.

### 9.2 Visual and Intuitive Learning

Users should be able to understand machine structures and mechanisms through clear visual representations and meaningful interactions.

### 9.3 Engineering Meaningfulness

Components, connections, assembly steps, and machine behaviour should represent meaningful engineering concepts rather than purely decorative interactions.

### 9.4 Reusability

Core product capabilities should be reusable across machine categories and engineering domains.

### 9.5 Extensibility

New machine experiences and component categories should be introduced without unnecessarily changing existing platform functionality.

### 9.6 Beginner Accessibility

The experience should be understandable to users who are new to engineering while preserving technical depth for advanced learners.

### 9.7 Consistency

Common operations such as component inspection, assembly, navigation, and simulation should behave consistently across the platform.

## 10. Product Scope and Development Direction

BuildMyMachine is intended to grow incrementally.

### Initial Development Focus

The initial development should establish the core platform capabilities:

- Foundational application experience.
- Interactive 3D workspace.
- Reusable component representation.
- Component library.
- Component placement and connections.
- Shared assembly capabilities.
- Guided Assembly.
- Free Assembly.
- Initial machine simulation capabilities.
- First complete machine experience: automatic bottle-capping machine.
- Second machine experience: electric motor, used to demonstrate platform reuse. *(Amendment A3)*
- User accounts, a backend, and saved user machine configurations. *(Amendment A1)*
- Playground ownership and administration, including versioned publishing. *(Amendment A1)*
- AI-assisted creation of user-created playgrounds from images, with human review and approval of every AI suggestion. *(Amendment A2)*

### Future Development

Future development may expand into:

- Additional industrial machines.
- Automotive systems.
- Robotic systems.
- More advanced mechanical assemblies.
- Expanded simulation capabilities.
- Additional learning experiences.
- Importing externally created 3D machine models.
- Additional engineering domains and machine categories.

Future capabilities must be introduced progressively based on product priorities.

## 11. Product Boundaries

The following boundaries must guide product development:

- BuildMyMachine is primarily an interactive engineering education, exploration, assembly, and simulation platform.
- It is not initially intended to replace professional CAD, industrial automation, or engineering analysis software.
- The platform should not claim manufacturing-grade accuracy for visual models unless that accuracy has been established and verified.
- Simulation capabilities must not be presented as physically accurate beyond their actual validated capabilities.
- Not every machine category or advanced feature is required in the initial release.
- Product development should prioritize a complete, functional initial experience rather than implementing many incomplete features.
- Future expansion should preserve the shared component and assembly concepts.
- Product decisions should prioritize user learning and engineering understanding.

## 12. Guidance for Kiro

Use this document as the persistent source of product direction when planning, designing, and implementing BuildMyMachine.

When working on the project:

1. Understand the complete product vision before proposing feature implementations.
2. Preserve the cross-domain nature of the platform.
3. Do not treat BuildMyMachine as an industrial-machinery-only application.
4. Keep Guided Assembly and Free Assembly as distinct user experiences supported by shared underlying capabilities.
5. Prioritize reusable components and extensible machine experiences.
6. Distinguish initial release requirements from future product possibilities.
7. Avoid introducing features that are unrelated to the product vision without discussing their relevance.
8. Do not independently change established product decisions.
9. Identify missing requirements, ambiguities, and technical dependencies before implementation.
10. Propose significant product or scope changes for review and approval before proceeding.

This document defines the product vision, objectives, capabilities, scope, and expected user experience. Detailed technical decisions, architecture, implementation standards, and testing requirements are defined in their respective Steering documents and feature specifications.

## 13. Approved Scope Amendments

The product owner reviewed and approved the following amendments to Section 10 on 2026-11-18. Original text of all other sections is unchanged.

- **A1 — Backend, accounts, saved machines.** These moved from "Future Development" to the initial release, together with playground ownership, administration and versioned publishing.
- **A2 — AI-assisted user-created playgrounds.** Creators can upload images, and AI suggests components, relationships, connection points, ranked fitments and assembly order. 3D models may be AI-generated. The creator must review, edit and approve every AI suggestion before publishing. Importing externally created 3D models stays in Future Development.
- **A3 — Second machine.** The electric motor is the second playground. It must reuse the existing platform with no core code changes.

Product boundaries that follow from these amendments (interpretations of Section 11, not new scope):

- AI-generated 3D geometry is visual only. It is not presented as accurate to a real machine.
- AI-generated components never gain simulation behaviour automatically. A creator must explicitly map a component to a supported platform-defined behaviour.
- AI confidence is advisory. It never replaces creator approval.
- All predefined and user-created playgrounds share one component ecosystem, 3D workspace, assembly engine, validation system and simulation architecture.
