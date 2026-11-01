# VST Chord Generator — Professional UI Polish Project

## Role and Context

Act as a senior C++/JUCE software engineer, audio plugin UI/UX designer, and technical design reviewer with experience building professional commercial music-production software.

**Repository:** https://github.com/jirwong/vst-chord-generator

I am developing an existing JUCE-based VST3 chord generator plugin. I want to elevate its visual quality, usability, consistency, and maintainability without unnecessarily changing its established design or functionality.

This is an **evolutionary UI polish project, not a ground-up redesign**.

The existing repository and its actual implementation must be the primary source of truth.

## 1. Project Objectives

The desired interface should feel like professional audio-production software: precise, purposeful, restrained, and visually distinctive.

Our target aesthetic combines:

- **Precision Instrument:** technical accuracy, strong visual hierarchy, refined typography, clear control states, and meticulous alignment.
- **Modern DAW Utility:** compact workflows, efficient use of space, predictable interactions, and clear parameter groupings.

Aim for a coherent product identity rather than a generic visual trend.

Avoid common AI-generated interface characteristics:

- Excessive cards, panels, and rounded containers.
- Gratuitous gradients, glowing accents, and decorative shadows.
- Arbitrarily large spacing and oversized controls.
- Inconsistent typography or visual hierarchy.
- Superficial styling that doesn't improve usability.
- Unnecessary replacement of existing UI components.

Visual changes must serve usability, clarity, consistency, or product identity.

## 2. Engineering Principles

All recommendations and eventual implementation should respect the following:

- Prioritise native JUCE Components, Graphics, and LookAndFeel.
- Preserve existing chord-generation, MIDI, parameter, preset, and host automation behaviour.
- Respect the current architecture unless there is a clear, documented reason to improve it.
- Prefer reusable components and centralised styling over scattered hardcoded values.
- Avoid introducing unnecessary dependencies or abstractions.
- Consider plugin window constraints, HiDPI scaling, resizing, and DAW integration.
- Keep rendering efficient and avoid unnecessary repainting.
- Distinguish visual polish from architectural refactoring and functional changes.

Do not recommend changes purely because they resemble currently fashionable UI patterns.

## 3. Six-Stage Project Roadmap

We will follow a controlled, approval-based process.

1. **Existing UI Audit:** Understand the current codebase, design, usability, and technical constraints.
2. **Visual Direction:** Explore and select an appropriate evolutionary design direction.
3. **Design System:** Establish typography, colour, spacing, geometry, and interaction standards.
4. **Visual Refinement Proposal:** Develop specific before/after improvements based on the real interface.
5. **Incremental Implementation:** Implement approved changes in small, reviewable steps.
6. **Validation:** Test visual consistency, rendering, interactions, performance, and host compatibility.

Each stage requires my explicit approval before moving to the next.

**Begin with Stage 1 only.**

---

# STAGE 1 — Repository and Existing UI Audit

## Mission

Conduct a thorough, evidence-based audit of the current VST Chord Generator interface and JUCE UI architecture.

**This stage is strictly read-only. Do not edit, generate replacement source files, refactor code, or start implementing visual changes.**

### A. Repository Investigation

Inspect the relevant repository files, including:

- PluginEditor and its supporting classes.
- LookAndFeel implementations and styling helpers.
- Custom JUCE Components.
- Layout and component bounds calculations.
- Graphics painting and rendering logic.
- Colours, fonts, spacing, geometry, and visual constants.
- Parameter attachments and plugin state interactions.
- Mouse, keyboard, drag, and focus interactions.
- Plugin editor resizing and scaling behaviour.
- Resources, assets, and existing screenshots or previews.

Identify the relevant source file paths and explain their responsibilities.

Do not assume conventional JUCE filenames or architecture without verifying them.

### B. Current UI Architecture

Document:

- The main UI component hierarchy.
- How the editor constructs and positions controls.
- Where visual styling decisions are implemented.
- Which components use native JUCE controls and which are custom.
- How the UI communicates with plugin parameters and state.
- Existing abstractions worth preserving.
- Any duplicated or unnecessarily complex styling logic.

Include a concise component hierarchy diagram where practical.

### C. Visual and UX Quality Assessment

Evaluate the existing interface against professional audio plugin standards.

Pay particular attention to:

1. Typography, font hierarchy, numerical readability, and label consistency.
2. Spacing, alignment, padding, and component dimensions.
3. Colour consistency, contrast, and visual emphasis.
4. Control affordances, discoverability, and interaction feedback.
5. Grouping, information density, and visual hierarchy.
6. Consistency between custom controls and standard JUCE components.
7. Window dimensions, resizing, clipping, and HiDPI behaviour.
8. Visual noise, unnecessary ornamentation, and inefficient space usage.
9. Workflow clarity for a user operating the plugin inside a DAW.

Evaluate the existing design fairly. Identify what already works well and should not be changed.

### D. Technical Risk Assessment

Investigate possible engineering risks associated with future UI refinements.

Focus on:

- Plugin parameter attachment stability.
- Synchronisation between UI and processor state.
- Component ownership and lifetime.
- Custom painting performance.
- Scaling and coordinate calculations.
- Layout dependencies and hardcoded dimensions.
- Host compatibility considerations.
- Accessibility and keyboard operation where applicable.

Distinguish confirmed issues from potential risks requiring testing.

### E. Evidence Standards

For every substantial finding:

- Reference the relevant source file and line number or code location.
- Explain the observed behaviour or implementation.
- Describe why it matters.
- Recommend an improvement appropriate to the existing architecture.
- Assign severity, expected impact, and implementation effort.
- Indicate whether the component should be preserved, refined, or consolidated.

Do not invent findings, screenshots, rendering defects, or code behaviour.

Source-code inspection alone is not proof of a visual rendering defect. If screenshots, a working build, or DAW testing are needed to verify something, explicitly identify that limitation.

If repository access is incomplete, state which files could not be inspected rather than assuming their contents.

---

# Required Stage 1 Deliverables

Prepare a structured audit report containing:

## 1. Executive Summary
A concise assessment of the current UI's strengths, weaknesses, and highest-impact polish opportunities.

## 2. Architecture Map
Relevant JUCE files, classes, component hierarchy, styling approach, and parameter interactions.

## 3. Existing Design Strengths
Elements and design decisions that should be preserved.

## 4. Prioritised UI Findings
A table containing:

| ID | Finding | Evidence / Source | Severity | User Impact | Effort | Recommendation |
|---|---|---|---|---|---|---|

Classify severity as Critical, Major, Minor, or Cosmetic.

## 5. Component-Level Assessment
Identify components that should be:

- Preserved unchanged.
- Visually refined.
- Standardised.
- Consolidated or architecturally improved.

## 6. Technical Risks
Document verified problems separately from possible issues requiring testing.

## 7. Recommended Polish Scope
Propose a practical scope divided into:

- High-priority refinements.
- Medium-priority improvements.
- Optional enhancements.
- Explicitly out-of-scope changes.

## 8. Stage 2 Preparation
Identify any screenshots, UI measurements, source files, or additional information needed to accurately develop the visual direction.

## 9. Audit Limitations
Explain what was inspected, what could not be verified, and what requires testing in a compiled plugin or DAW.

---

# Important Operating Rules

- Do not modify the repository.
- Do not create commits or pull requests.
- Do not start redesigning the UI.
- Do not generate implementation code.
- Do not introduce new features.
- Do not assume the current interface is poorly designed.
- Do not recommend wholesale replacement of existing components without justification.
- Do not automatically proceed to Stage 2.

**Success means producing an accurate, actionable understanding of the existing plugin and a justified, prioritised plan for polishing it.**

At the end of Stage 1, provide the audit for my review and stop. Wait for my explicit approval before proceeding.
