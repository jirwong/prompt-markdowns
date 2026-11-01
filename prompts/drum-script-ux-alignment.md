# DrumScript — UI Alignment with ChordScript

## Role and context

Act as a senior C++/JUCE engineer, audio plug-in UI designer and technical reviewer.

I want to refine DrumScript so it looks and feels like part of the same product family as ChordScript. DrumScript’s core functionality is implemented, and basic MIDI playback to Atlas has been tested successfully. Its current interface differs substantially from the approved UI concept.

This project should bring the native interface into alignment with the approved layout and ChordScript’s design system.

## Projects and references

DrumScript:
`C:\dev\vst\vst-drumscript`

ChordScript:
`C:\dev\vst\vst-chord-generator`

Inspect these references:

- ChordScript’s `docs/UI_DESIGN_SYSTEM.md`.
- ChordScript’s actual native styling, LookAndFeel, controls, layout and painting code.
- ChordScript’s native screenshots and approved references linked from its design system.
- DrumScript’s `docs/design-references/drumscript-layout-concept-2026-10-31.png` and corresponding HTML concept.
- DrumScript’s `docs/UI_DESIGN_SYSTEM.md`.
- DrumScript’s approved specifications, current editor, grid, overview and supporting components.

Verify current file locations, branch and repository status before relying on them. Preserve unrelated local changes.

## Design authority

**ChordScript’s current native interface defines the visual identity.** Follow its typography, colours, surfaces, borders, geometry, spacing, control appearance, hover states and keyboard-focus treatment.

**The approved DrumScript concept defines the intended arrangement.** Aim for a close match to its composition and hierarchy:

1. Header and transport status.
2. Pattern text and parsing controls.
3. Pattern settings.
4. Drum sequencer and navigation.
5. Selected-hit controls.
6. MIDI actions and footer.

Allow justified layout adjustments where actual controls, native font metrics, readability, accessibility or host-window constraints require more space. Explain material adjustments before implementation.

When references differ, follow ChordScript for visual identity and the DrumScript concept for arrangement. Identify material conflicts for review.

The concept contains illustrative data and controls. Do not interpret these as approval to add features or change defaults.

## Objectives and constraints

Apply the design system throughout the interface. Matching colours and fonts alone is insufficient: controls, panels, labels, spacing, states and layout must work together.

Preserve existing:

- MIDI generation and timing.
- Text syntax and parsing.
- Auto and Manual workflows.
- Grid editing and conflict handling.
- Pattern lengths, lane loops, resolutions, nudges and bursts.
- Project state and restoration.
- Export, clipboard and drag behaviour.

Use native JUCE components and rendering. Adapt suitable ChordScript styling into DrumScript while keeping DrumScript independently buildable. Do not modify ChordScript or introduce a build dependency on it.

Prefer reusable styling and clear layout rules over scattered hardcoded values. Distinguish visual changes, architectural refactoring and functional changes.

Preserve useful existing components and interactions. Explain any proposed replacement and its benefit.

Follow applicable AGENTS.md instructions. Never use permanent deletion commands; use trash-cli if file removal is necessary.

## Process and approval gates

Work through these six stages:

1. Read-only UI and architecture audit.
2. Visual alignment proposal.
3. DrumScript design system adapted from ChordScript.
4. Detailed refinement proposal and implementation plan.
5. Incremental implementation.
6. Native visual, interaction and DAW validation.

Each stage requires my explicit approval before the next begins.

**Begin with Stage 1 only.**

At the end of each stage, present the deliverables, identify unresolved decisions and stop for review. Approval of a stage does not authorise skipping later approval gates.

Keep explanations in clear, simple technical English.

---

## Stage 1 — Read-only UI and architecture audit

### Purpose

Establish what exists, where it differs from the references, and what can be reused safely.

### Investigation

Compare DrumScript’s current implementation with ChordScript and the approved DrumScript concept.

Investigate:

- Component hierarchy and styling ownership.
- Standard JUCE controls and custom painting.
- Reusable styling and layout abstractions.
- Typography, colours, surfaces, geometry and interaction states.
- Layout, spacing, alignment and information hierarchy.
- Text editing, settings, grid, selected-hit controls and MIDI actions.
- Scaling, resizing, scrolling, clipping and grid readability.
- Keyboard operation, focus and accessibility.
- UI/state synchronisation, component lifetime and rendering risks.
- ChordScript styling and components suitable for adaptation.

Use available native screenshots or the running plug-in where practical. Identify which build is being viewed.

Source inspection alone does not prove a visible rendering defect. If native evidence is unavailable, complete the source-based audit and identify the specific screenshots or checks still required.

### Deliverables

Present:

1. A concise assessment of the current UI and largest gaps.
2. A component and styling architecture map.
3. Existing strengths and behaviour worth preserving.
4. A prioritised findings table with evidence, severity, user impact, effort and recommendation.
5. A mapping from relevant ChordScript rules or components to DrumScript.
6. Recommended refinement scope and implementation order.
7. Confirmed technical risks, possible risks requiring testing and audit limitations.
8. Questions that must be resolved before the visual proposal.

Reference actual file paths and code locations. Distinguish current implementation, approved guidance and proposed changes.

### Boundary

Do not edit repository files, generate replacement components, create commits or PRs, or implement visual changes.

Present the audit in the chat and stop for review.

---

## Stage 2 — Visual alignment proposal

### Purpose

Agree on how DrumScript should look before specifying implementation details.

### Work

Develop a focused proposal based on the approved Stage 1 findings.

Show how the ChordScript visual identity applies to DrumScript’s:

- Header, title and status.
- Pattern text area and parsing controls.
- Settings groups.
- Sequencer frame, timeline and lane labels.
- Hits, velocity indication, selection and playhead.
- Selected-hit controls.
- MIDI actions, footer and UI-scale control.
- Diagnostics, pending drafts and conflict decisions.

Use annotated comparisons and a clearly labelled mockup or interactive layout preview. State whether each image is a native screenshot, browser prototype or other design illustration.

Aim for a close match to the existing DrumScript concept. Present a recommended direction rather than several unrelated redesigns. Show alternatives only for consequential unresolved choices.

Include representative states: text expanded and collapsed, a selected hit, and a diagnostic or Manual conflict.

Account for the actual implemented controls. Explain where additional space or different grouping is necessary.

### Deliverables

Present:

1. The recommended visual direction.
2. An annotated layout showing the main sections.
3. A current-versus-proposed comparison where evidence is available.
4. Proposed control groupings and visibility rules.
5. Layout adjustments from the original concept and their reasons.
6. Remaining choices requiring my decision.

### Boundary

Mockups are design artefacts. Do not change production UI code.

Stop for approval of the visual direction.

---

## Stage 3 — DrumScript design system

### Purpose

Turn the approved direction into precise, reusable rules for native implementation.

### Work

Adapt ChordScript’s design system to DrumScript. Document the source of inherited rules and explain any DrumScript-specific additions.

Specify:

- Colour roles and exact values.
- Font faces, weights and logical sizes.
- Heading, label, body, numeric and pattern-text hierarchy.
- Spacing, padding, alignment and geometry.
- Surface, border, radius and depth treatments.
- Buttons, text actions, toggles, dropdowns, sliders and value fields.
- Text-editor appearance and selection.
- Idle, hover, pressed, selected, disabled, focus and error states.
- Grid subdivisions, beat/bar emphasis and lane boundaries.
- Hit, velocity, burst, selection and playhead appearance.
- Scrollbars, overview navigation and page indicators.
- Disclosure, diagnostics and conflict presentation.
- Scaling, resizing and minimum usable dimensions.

Use semantic style roles so future components can follow the same rules.

Document how styling should be shared through native JUCE LookAndFeel, painting helpers and component-level rules. Keep the approach proportionate to the project.

### Deliverables

Produce a reviewable design-system document containing:

1. Design principles and reference priorities.
2. Measured tokens and component rules.
3. Grid-specific visual rules.
4. Layout and scaling rules.
5. Interaction and accessibility requirements.
6. The proposed styling architecture.
7. Examples showing how the rules apply.

Distinguish approved requirements from implementation suggestions and unresolved choices.

### Boundary

Documentation and design examples are allowed. Do not implement the production styling yet.

Stop for approval of the design system.

---

## Stage 4 — Detailed refinement proposal and implementation plan

### Purpose

Translate the approved design into small, reviewable engineering tasks.

### Work

Map each proposed change to the actual files and components it affects.

Define the layout for:

- Normal and compact window sizes.
- Text expanded and collapsed.
- Diagnostic and conflict states.
- Selected-hit editing.
- Supported UI scales.
- One-, two- and eight-bar patterns.

Explain which areas resize, scroll, collapse or retain fixed dimensions. Avoid solving one section’s expansion by making another section unusably small.

Plan a small initial native implementation that proves the styling approach and font/layout measurements before converting the whole editor.

Group later tasks into coherent sections, with clear dependencies and acceptance criteria.

### Deliverables

Present:

1. The final annotated refinement proposal.
2. Files and components to change.
3. Styling reuse and any justified refactoring.
4. Ordered implementation tasks.
5. Behaviour that each task must preserve.
6. Appropriate automated and manual checks.
7. Screenshot comparisons and visual acceptance criteria.
8. Risks, exclusions and unresolved decisions.

Visual acceptance must describe observable results, not simply “looks polished” or “uses the design system.”

### Boundary

Do not begin production implementation until I approve this plan.

---

## Stage 5 — Incremental implementation

### Purpose

Implement the approved plan while preserving a working plug-in.

### Work

Follow the approved task order. Begin with the agreed native styling sample and check that it supports the visual direction before expanding it.

Centralise shared styling and apply it consistently. Use native layout and drawing; do not use a mockup image as the interface background.

For each task:

- Keep the changes focused.
- Build the relevant targets.
- Run checks appropriate to the change.
- Inspect native rendering where possible.
- Compare the result with the approved references.
- Review interactions and state handling affected by the change.
- Report material deviations promptly.

Use automated tests where they protect meaningful behaviour or calculations. Use native screenshots and interaction checks for appearance.

Do not silently introduce features, alter musical behaviour or make major unapproved layout changes.

### Deliverables

Provide:

1. The implemented changes.
2. A concise explanation of what changed and why.
3. Native screenshots or clearly stated visual-verification gaps.
4. Build and test results.
5. Any deviations, known issues or remaining work.
6. Updated design-system documentation reflecting the approved implementation.

Keep commits reviewable. Installation, pushing, PR creation, merging and release require separate authorisation unless I have explicitly included them in the current scope.

Stop for review before final acceptance.

---

## Stage 6 — Native visual, interaction and DAW validation

### Purpose

Verify the completed interface in the actual plug-in and host environment.

### Validation

Check:

- Alignment with the approved visual direction.
- Consistent fonts, colours, spacing, surfaces and control states.
- Readability and absence of clipping or overlap.
- Expanded, collapsed, diagnostic and conflict layouts.
- Supported UI scales and usable window sizes.
- One-, two- and eight-bar navigation.
- Sixteen-lane readability, lane loops and mixed resolutions.
- Hit selection, velocity editing, bursts and timing controls.
- Playhead animation and automatic page following.
- Mouse, keyboard, focus and accessibility behaviour.
- Auto and Manual text/grid workflows.
- Project save, close and reopen.
- MIDI export, drag and clipboard actions.
- Rendering responsiveness and unnecessary repainting.
- Available DAW and host-scaling combinations.

Use representative patterns and repeatable states for screenshot comparisons. Record the build, host, scale and window size used.

Distinguish confirmed results from unperformed checks. Do not claim host or screen-reader acceptance from source inspection or automated component tests.

### Deliverables

Present:

1. A validation matrix with pass, fail or not tested.
2. Native screenshots of the key states.
3. Confirmed defects and their severity.
4. Outstanding host, accessibility or scaling checks.
5. Documentation updates and known limitations.
6. A recommendation on readiness for normal use.

Fix defects within the approved scope and repeat the affected checks. Bring material design changes back for approval.

Stop after presenting the validation results. Do not automatically publish, install, merge or release.

---

## Start now

Begin Stage 1 only.

Inspect the repositories and available references, perform the read-only audit, present the findings and stop for my review.
