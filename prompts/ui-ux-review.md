# DrumScript — UI Alignment with ChordScript

Act as a senior C++/JUCE engineer and audio plug-in UI designer.

I want to refine DrumScript so it looks and feels like part of the same product family as ChordScript. DrumScript’s core functionality is implemented, and basic MIDI playback to Atlas has been tested successfully. Its current interface differs substantially from the approved UI concept.

## Projects and references

DrumScript:
`C:\dev\vst\vst-drumscript`

ChordScript:
`C:\dev\vst\vst-chord-generator`

Inspect:

- ChordScript’s `docs/UI_DESIGN_SYSTEM.md`.
- ChordScript’s actual native styling, LookAndFeel, controls, layout and painting code.
- ChordScript’s native screenshots and approved references linked from its design system.
- DrumScript’s `docs/design-references/drumscript-layout-concept-2026-10-31.png` and corresponding HTML concept.
- DrumScript’s `docs/UI_DESIGN_SYSTEM.md`, approved specifications and current UI implementation.

Verify current file locations, repository status and branch before relying on them. Preserve unrelated local changes.

## Design authority

**ChordScript’s current native interface defines the visual identity.** Follow its typography, colours, surfaces, borders, geometry, spacing, control appearance, hover states and keyboard-focus treatment.

**The approved DrumScript concept defines the intended arrangement.** Aim for a close match to its composition and hierarchy: pattern text, pattern settings, drum sequencer, selected-hit controls and MIDI actions.

Allow justified layout adjustments where actual controls, native font metrics, readability, accessibility or host-window constraints require more space. Explain these adjustments before implementation.

When references differ, follow ChordScript for visual identity and the DrumScript concept for arrangement. Identify material conflicts for review.

The concept contains illustrative data and controls. Do not treat these as approval to add features or change defaults.

## Objectives and engineering constraints

Apply the design system throughout the interface. Matching colours and fonts alone is insufficient: controls, panels, labels, spacing, states and layout must work together.

Preserve existing MIDI generation, text parsing, grid editing, conflict handling, timing, project state, export and drag behaviour.

Use native JUCE components and rendering. Adapt suitable ChordScript styling into DrumScript while keeping DrumScript independently buildable. Do not modify ChordScript or introduce a build dependency on it.

Prefer reusable styling and clear layout rules over scattered hardcoded values. Distinguish visual changes from architectural refactoring and functional changes.

Keep the interface precise, restrained and suitable for music production. Preserve useful existing components and interactions.

Follow applicable AGENTS.md instructions. Never use permanent deletion commands; use trash-cli if file removal is necessary.

## Stages and approval gates

1. Read-only UI and architecture audit.
2. Visual alignment proposal.
3. DrumScript design system adapted from ChordScript.
4. Detailed refinement proposal and implementation plan.
5. Incremental implementation.
6. Native visual, interaction and DAW validation.

Each stage requires my explicit approval before the next begins.

**Begin with Stage 1 only.**

## Stage 1 — Read-only audit

Compare DrumScript’s current implementation with ChordScript and the approved DrumScript concept.

Investigate:

- Component hierarchy, styling ownership and reusable abstractions.
- Standard JUCE controls and custom painting.
- Typography, colours, surfaces, geometry and interaction states.
- Layout, spacing, alignment and information hierarchy.
- Text editing, settings, grid, selected-hit controls and MIDI actions.
- Scaling, resizing, scrolling, clipping and grid readability.
- Keyboard operation, focus and accessibility.
- UI/state synchronisation, component lifetime and rendering risks.
- ChordScript styling and components suitable for adaptation.

Use available native screenshots or the running plug-in where practical. Identify which build is being viewed. Source inspection alone does not prove a visible rendering defect.

If native evidence is unavailable, complete the source-based audit and identify the specific screenshots or checks still required. Do not invent visual findings or claim unperformed validation.

## Stage 1 deliverables

Present:

1. A concise assessment of the current UI and largest gaps.
2. A component and styling architecture map.
3. Existing strengths and behaviour worth preserving.
4. A prioritised findings table with evidence, severity, user impact, effort and recommendation.
5. A mapping from relevant ChordScript design rules or components to DrumScript.
6. Recommended refinement scope and implementation order.
7. Confirmed technical risks, possible risks requiring testing, and audit limitations.
8. Questions that must be resolved before the visual proposal.

Reference actual file paths and code locations. Distinguish current implementation, approved guidance and proposed changes.

During Stage 1, do not edit repository files, generate replacement components, create commits or PRs, or implement visual changes.

Present the audit in the chat, then stop for my review.
