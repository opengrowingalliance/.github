# OGP Reframing Review Report

**Date**: 2026-02-09
**Scope**: Complete documentation reframing to align with founder's original vision
**Status**: Ready for review before commit

---

## Executive Summary

**Transformation**: From "universal agriculture protocol" to "Kubernetes-inspired cannabis CEA automation for home growers, guided by solarpunk principles"

**Documents Modified**: 7 total
- 4 Public-facing (README, Manifesto, SCOPE, ALLIANCE)
- 2 Technical specs (Root, Recipe)
- 3 Internal (Memory-bank)

**New Documents Created**: 2
- `SCOPE.md` - Anti-scope-creep framework
- `ALLIANCE.md` - Organizational manifesto

---

## 1. README.md (Main Repository)

### Critical Changes

**BEFORE**:
- Scope: "diverse plant species and growing methods"
- Target: Implied universal agriculture
- Vision: Mixed current deliverables with aspirational goals

**AFTER**:
- **First line**: "An open protocol for cannabis controlled environment agriculture (CEA)"
- **Current Scope section**: Cannabis, CEA, home growers, <$1000 systems
- **Long-Term Vision section**: Solarpunk principles clearly labeled "Aspirational"
- **New section**: "Why Cannabis CEA First?" - 5 strategic reasons

### Key Additions
- Explicit target: "home growers using affordable hardware (target: <$1000)"
- Roadmap honesty: Realistic Q1-Q4 2026 deliverables
- Clear separation: "What We're Building Now" vs. "Long-Term Vision"

### Removals
- "Small farmer in Brazil" example (from current deliverables)
- "Indigenous community" use case (requires co-design)
- "Democratize agriculture" framing (too broad for now)

**Status**: ✅ Execution focus + solarpunk vision balanced

---

## 2. profile/README.md (OGP Manifesto)

### Critical Changes

**BEFORE**:
- Title implied universal solution
- Examples: Small farmers globally, indigenous communities
- Tone: Revolutionary, universal transformation

**AFTER**:
- **Title**: "Growing Personal Sovereignty, One Garden at a Time"
- **Subtitle**: "Starting with controlled environment agriculture for home cannabis growers"
- **Tone**: Inspiring but grounded, "one basement, one tent, one grower at a time"

### Key Transformations
- "Small farmer in Brazil" → "Home grower in their basement"
- "Democratize agriculture" → "Personal cultivation sovereignty"
- "Universal solution" → "Focused effort to prove open protocols work"

### Solarpunk Authenticity Preserved
- Principle 5: "Honest Sustainability Through Measured Impact" - admits CEA energy use
- Knowledge commons WITHOUT extractive claims
- "Judge us by what we ship, not what we write" (closing line)

### Indigenous Knowledge
- **BEFORE**: Claimed as current use case
- **AFTER**: "requires co-design with those communities" (honest about limitation)

**Status**: ✅ Authentic solarpunk without greenwashing

---

## 3. SCOPE.md (NEW DOCUMENT)

### Purpose
Explicit scope definition to prevent future expansion creep (like what happened before)

### Structure
1. **Current Focus (2026-2027)**: Cannabis CEA, home growers, <$1000
2. **Explicitly Out of Scope**:
   - Field agriculture
   - Commercial scale
   - Universal crops
   - Indigenous knowledge (requires expertise we don't have)
3. **Future Possibilities (Aspirational)**: Other CEA crops, conditional on validation
4. **Expansion Criteria**: Evidence-based (50+ users, 3+ grow cycles, community demand)
5. **Solarpunk Principles**: Design drivers, not promises

### Anti-Scope-Creep Features
- **Versioning**: 1.x = clarifications, 2.x = scope changes
- **Red Flags**: Section identifying scope creep patterns
- **Decision Framework**: How to evaluate expansion requests
- **Community Process**: 2-week comment period for changes

**Status**: ✅ Framework to keep project focused

---

## 4. ALLIANCE.md (NEW DOCUMENT)

### Purpose
Separate organizational manifesto (Open Growing Alliance) from protocol manifesto (OGP)

### Key Distinctions
- **Alliance**: Who we are, how we govern, how to participate
- **OGP**: What we're building (cannabis CEA automation)

### Critical Sections

**About the Alliance**:
- "We are not the protocol. We are its caretakers."
- Honest about early stage: "small team, focused scope, building foundations"
- Not pretending to be established organization

**Governance**:
- Current: Founder-led, public discussions
- Evolution: Working groups, RFC process, community consensus
- Commitment: Open source forever, vendor neutral, community can fork

**Membership**:
- "Everyone. No barriers, no gatekeeping."
- "No membership fees, no applications, no approval processes"
- "Participation is contribution"

**Values**:
1. Community Over Corporate
2. Radically Open, Permanently
3. Honest Scope, Always
4. Solarpunk Pragmatism (not utopianism)
5. Attribution and Credit

**Status**: ✅ Clear org identity separate from protocol

---

## 5. ogp-specifications/v0.2/ogp-root-spec.md

### Critical Changes

**Section 1.2 - OGP Vision**:
- **ADDED**: "OGP v0.2 specifies protocols for Controlled Environment Agriculture (CEA), with initial focus on cannabis cultivation"
- **ADDED**: "Initial Scope: The v0.2 specification focuses on cannabis CEA to establish proven patterns"

**Section 2.3 - Out of Scope (NEW)**:
Explicit exclusions:
- Field agriculture
- Universal portability claims (recipes are "calibratable templates")
- Non-CEA crops (in v0.2)
- Immediate commercial scale
- Real-time cross-system coordination
- Soil ecology-dependent methods

**Section 2.2 - Design Goals**:
- Updated portability claims: "recipes require calibration for local hardware and genetic variability"
- Not plug-and-play solutions

**Section 4.2 - Use Cases**:
Completely rewritten focusing on:
- Home cannabis cultivation (1-10 plants)
- Small-scale cannabis production
- Cannabis research
- CEA infrastructure types (tents, rooms, greenhouses, chambers)
- Future expansion path with validation requirements

**Status**: ✅ Technical honesty about scope + architecture preserved

---

## 6. ogp-specifications/v0.2/recipe/ogp-recipe-spec.md

### Critical Changes

**Introduction**:
- Scope: "cannabis cultivation in Controlled Environment Agriculture (CEA) settings"
- Portability: "calibratable templates rather than plug-and-play solutions"

**Phase Types**:
- **BEFORE**: Generic plant phases
- **AFTER**: Cannabis-specific (Germination, Seedling, Vegetative, Pre-Flower, Early/Mid/Late Flowering)
- Added clone propagation alternatives

**Cannabis-Specific Parameters (NEW Section)**:
- Photosynthetic parameters (PPFD, DLI, spectrum ratios)
- Nutrient parameters (EC, PPM, pH, N-P-K)
- Environmental stability (VPD, temp differential, humidity ramping)
- Genetic considerations (photoperiod sensitivity, flowering time, stretch factor)

**Calibration Requirements (NEW Section)**:
Comprehensive section covering:
- Hardware calibration (light mapping, sensor validation, airflow, irrigation)
- Genetic calibration (strain variation, phenotype expression, training methods)
- Environmental calibration (baseline climate, space characteristics)
- Calibration workflow guidance

**Example Recipe**:
- **BEFORE**: Generic plant
- **AFTER**: "Northern Lights" cannabis strain with:
  - Real cannabis parameters (VPD, DLI, EC ranges)
  - Strain-specific notes (heavy feeder, minimal stretch, dense flowers)
  - Detailed flowering phase with progressive humidity reduction
  - Trichome-based harvest indicators
  - Flush period for quality improvement
  - Calibration notes for first-time growers

**Status**: ✅ Cannabis-focused with technical rigor

---

## 7. memory-bank/* (3 files)

### projectbrief.md
- Core objectives → cannabis CEA specific
- Target users → home growers primary
- Key components → cannabis parameters (PPM, DLI, VPD, grow phases)
- Governance → cannabis community representation
- Added clear MVP status and aspirational long-term vision

### productContext.md
- Problem statement → cannabis cultivation challenges
- Target users → home growers as primary, aspirational future users separated
- Key benefits → cannabis CEA context
- Ecosystem vision → cannabis community
- Solarpunk vision → current/aspirational split

### activeContext.md
- Development focus → cannabis CEA MVP
- Immediate priorities → cannabis-specific tasks
- Open questions → home-scale cannabis cultivation
- Pending decisions → home grower context
- Next steps → Phased (Specification → Implementation → Validation → Expansion)

**Status**: ✅ Internal context aligned with refocused scope

---

## Summary of Transformations

### Scope Changes
| Aspect | Before | After |
|--------|--------|-------|
| **Domain** | Universal agriculture | Cannabis CEA only |
| **Users** | Small farmers globally | Home growers, hobbyists |
| **Cost** | Unspecified | <$1000 target |
| **Environment** | All growing contexts | Controlled environments |
| **Expansion** | Implied universal | Conditional on validation |

### Messaging Changes
| Concept | Before | After |
|---------|--------|-------|
| **Vision** | Democratize agriculture | Personal cultivation sovereignty |
| **Solarpunk** | Current features | Guiding principles (aspirational) |
| **Portability** | Plug-and-play recipes | Calibratable templates |
| **Indigenous** | Current use case | Requires co-design (not claiming) |
| **Scale** | All agriculture | Cannabis CEA first, expand later |

### Document Structure
| Type | Before | After |
|------|--------|-------|
| **Organization** | Mixed with protocol | Separate (ALLIANCE.md) |
| **Scope** | Implicit | Explicit (SCOPE.md) |
| **Vision** | Mixed with execution | Separated ("Now" vs "Aspirational") |
| **Examples** | Generic plants | Cannabis-specific |

---

## What Stayed the Same

### Preserved Elements
✅ **Solarpunk principles** - As guiding design philosophy
✅ **Kubernetes architecture** - Control/data plane separation
✅ **Declarative recipes** - Desired state model
✅ **Hardware abstraction** - With honest limitations
✅ **Community governance** - Open source, vendor neutral
✅ **Knowledge commons** - Recipe sharing with attribution
✅ **Long-term vision** - May expand to other crops (aspirational)

### Technical Architecture
✅ **Recipe format** - YAML-based declarative
✅ **Control loops** - Reconciliation model
✅ **Hardware abstraction layer** - With richer capability specs
✅ **Git-style versioning** - Fork, modify, share
✅ **Modular specifications** - Root, Core, Hardware, Recipe

---

## Files Changed (Git Status)

### Modified Files
- `README.md`
- `profile/README.md`
- `ogp-specifications/v0.2/ogp-root-spec.md`
- `ogp-specifications/v0.2/recipe/ogp-recipe-spec.md`
- `memory-bank/projectbrief.md`
- `memory-bank/productContext.md`
- `memory-bank/activeContext.md`

### New Files
- `SCOPE.md`
- `ALLIANCE.md`
- `REFRAMING-REVIEW.md` (this document)

### Validation Documents (Not for commit)
- `VALIDATION-SYNTHESIS.md`
- `FOUNDER-ORIGIN-REASSESSMENT.md`
- `KUBERNETES-ARCHITECTURE-EVALUATION.md`
- `MVP-IMPLEMENTATION-DESIGN.md`
- `PRIOR-ART-ANALYSIS.md`
- `SCOPE-EXPANSION-FAILURE-ANALYSIS.md`
- `SUSTAINABILITY-CRITIQUE.md`
- `validation-indigenous-knowledge.md`
- `validation/agricultural-viability-assessment.md`

---

## Review Checklist

Before committing, verify:

### Scope Clarity
- [ ] Cannabis CEA explicitly stated in all public docs?
- [ ] Home growers as primary target clear?
- [ ] <$1000 cost target mentioned?
- [ ] CEA environment requirement clear?

### Solarpunk Authenticity
- [ ] Principles preserved as design drivers?
- [ ] Long-term vision labeled "aspirational"?
- [ ] No greenwashing or techno-solutionism?
- [ ] Honest about energy use (CEA reality)?

### Overpromises Removed
- [ ] "Small farmer in Brazil" removed from current deliverables?
- [ ] "Indigenous knowledge" properly caveated?
- [ ] "Democratize agriculture" reframed to "personal sovereignty"?
- [ ] Universal agriculture claims removed?

### Technical Honesty
- [ ] Recipes described as "calibratable templates"?
- [ ] Portability caveats clear (not plug-and-play)?
- [ ] "Out of Scope" sections added?
- [ ] Cannabis-specific parameters included?

### Organizational Clarity
- [ ] Alliance (org) separated from OGP (protocol)?
- [ ] Governance structure transparent?
- [ ] Early stage honestly acknowledged?
- [ ] Community participation clear?

---

## Recommendation

**APPROVED FOR COMMIT** with the following notes:

1. **Alignment**: All documents now reflect founder's original vision (Kubernetes for cannabis CEA)
2. **Consistency**: Messaging consistent across all documents
3. **Honesty**: Scope boundaries clear, overpromises removed
4. **Preservation**: Solarpunk principles maintained as guiding philosophy
5. **Separation**: Alliance (org) vs OGP (protocol) clarified

**Suggested commit message**:
```
Refocus OGP on cannabis CEA with solarpunk principles

- Update README.md: Cannabis CEA scope, home growers target
- Reframe manifesto: Personal sovereignty, honest scope
- Create SCOPE.md: Anti-scope-creep framework
- Create ALLIANCE.md: Organizational manifesto (org vs protocol)
- Update specs: Cannabis-specific examples, calibration requirements
- Update memory-bank: Cannabis CEA development focus

Aligns with founder's original vision (Kubernetes for cannabis CEA)
while preserving solarpunk as guiding principles.
```

---

## Next Steps

1. **Review this document** - Read all sections above
2. **Check individual files** - Read modified docs if desired
3. **Approve or request changes** - Let team know if revisions needed
4. **Commit when ready** - Stage and commit all changes
5. **Push to remote** - Share refocused OGP with community

**Team Status**: All 6 reframing agents idle, awaiting your decision.
