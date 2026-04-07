# Open Growing Alliance - Project Scope

**Version**: 1.0
**Last Updated**: 2026-02-09
**Status**: Active boundary definition

## Purpose of This Document

This document exists to prevent scope creep and maintain focus. OGP began as a cannabis CEA project and expanded to "universal agriculture" without validation. This caused resource dilution and mission drift. These boundaries ensure we validate our core thesis before expanding.

## Current Focus (2026-2027)

### What We're Building NOW

**Target Domain**: Cannabis cultivation in controlled environment agriculture (CEA)

**Target Users**:
- Home growers (personal/medical use)
- Small-scale hobbyists
- DIY automation enthusiasts

**Target Systems**:
- Budget: <$1000 initial investment
- Scale: 1-4 plant systems
- Environment: Grow tents, closets, small dedicated spaces
- Automation: Entry-level to intermediate (Arduino/ESP32)

**Core Deliverables**:
1. Open hardware designs for cannabis CEA
2. Sensor/actuator protocols optimized for small grows
3. Community-validated growing recipes
4. Documentation for home growers
5. Reference implementations (hardware + firmware)

### Why This Focus?

- **Tractable**: Small enough to validate in 12-18 months
- **Underserved**: Home cannabis growers lack open, affordable automation
- **Legal Context**: Expanding legalization creates growing market
- **Technical Fit**: CEA constraints manageable at small scale
- **Community**: Existing enthusiast base for open-source growing tools

## Explicitly Out of Scope (For Now)

### NOT Building in 2026-2027

**Field Agriculture**:
- Outdoor row crops
- Precision agriculture for farms
- Soil-based systems at scale
- Weather-dependent growing

**Commercial Scale CEA**:
- Greenhouse operations
- Vertical farms
- Multi-room facilities
- Industrial automation

**Universal Crop Support**:
- Tomatoes, lettuce, peppers (unless directly applicable to cannabis)
- Specialty crops
- Crop rotation systems
- Multi-crop facilities

**Indigenous Knowledge Integration**:
- Traditional ecological knowledge (TEK) systems
- Ethnobotanical databases
- Community governance models
- (These require expertise we don't yet have and risk extractive dynamics)

**Advanced Features**:
- Machine learning yield optimization
- Computer vision phenotyping
- Blockchain traceability
- IoT cloud platforms

### Why These Are Out

Not because they're bad ideas. Because:
1. **Resource Constraint**: Small team, limited budget
2. **Validation First**: Prove core thesis before expanding
3. **Avoid Dilution**: General solutions often serve no one well
4. **Prevent Overengineering**: Perfect is the enemy of good enough
5. **Respect Complexity**: Some domains (TEK, commercial ag) require expertise we lack

## Future Possibilities (Aspirational)

### What MIGHT Come Later

**IF** we successfully validate cannabis CEA (50+ active users, 3+ growth cycles, documented results):

**Adjacent CEA Crops**:
- Herbs (basil, mint, cilantro)
- Microgreens
- Leafy greens (lettuce, kale)
- _Criteria_: Similar environmental controls, proven demand from existing users

**Scale Expansion**:
- Small commercial grows (4-12 plants)
- Community garden CEA systems
- Educational institution deployments
- _Criteria_: Reference customers, validated economics, support capacity

**Technology Evolution**:
- Mobile app interfaces
- Cloud data logging (opt-in, privacy-preserving)
- Community recipe marketplace
- _Criteria_: User requests, privacy-first design, sustainable funding

**Community Governance**:
- Indigenous knowledge partnerships (if invited, with consent protocols)
- Regional growing guilds
- Standards body formation
- _Criteria_: Community-led, equitable governance, no extraction

### What We'll NEVER Build

- Proprietary/closed systems
- Surveillance agriculture
- IP-encumbered genetics databases
- Systems requiring vendor lock-in
- Extractive data platforms

## Expansion Criteria

### How We Decide to Grow Scope

Expansion requires EVIDENCE of:

1. **Validation**: Current scope demonstrably working
   - 50+ active deployments
   - 3+ complete growth cycles
   - Community documentation contributions

2. **Demand**: User-driven requests (not founder assumptions)
   - GitHub issues from 10+ users
   - Forum discussions with participation
   - Community proposals with champions

3. **Resources**: Sustainable capacity to support
   - Maintainer availability
   - Documentation bandwidth
   - Support infrastructure

4. **Alignment**: Fits solarpunk principles (see below)
   - Open protocols maintained
   - Community governance preserved
   - Appropriate technology philosophy

### Red Flags (Scope Creep Indicators)

- "Wouldn't it be cool if..." without user validation
- "Everyone needs this" without evidence
- "Quick addition" that requires new expertise
- "Strategic partnership" requiring pivot
- "Grant opportunity" pulling us off-mission

## Solarpunk Principles (Non-Negotiable)

### What Keeps Us Aligned

**Open Protocols**:
- No proprietary formats
- Documented interfaces
- Reference implementations
- Interoperability by default

**Community Governance**:
- Open decision-making
- Contributor recognition
- Consensus-seeking (not consensus-required)
- Transparent roadmaps

**Appropriate Technology**:
- Simple before complex
- Repairable over disposable
- Local-first over cloud-dependent
- Accessible over exclusive

**Ecological Integrity**:
- Energy efficiency prioritized
- Material reuse enabled
- E-waste minimization
- Closed-loop thinking

**Equity & Access**:
- Low-cost viable paths
- Global South accessibility
- Language/cultural inclusivity
- No extractive dynamics

## Using This Document

### For Contributors

Before proposing features, check:
1. Is this in "Current Focus"? → Proceed
2. Is this in "Out of Scope"? → Explain why it's changed OR table for future
3. Is this in "Future Possibilities"? → Wait for validation OR make case for early inclusion
4. Is this against "Principles"? → Redesign or decline

### For Maintainers

Use this doc to:
- Politely decline scope-expanding requests
- Redirect energy to core thesis
- Set expectations with new contributors
- Justify resource allocation

### For Users

This doc tells you:
- What to expect in 2026-2027
- What NOT to expect soon
- How to advocate for future features
- How to align contributions with mission

## Document Maintenance

### When to Update

**Version Bumps**:
- 1.x: Clarifications, examples, formatting (no scope change)
- 2.x: Moving items between sections (scope change)
- 3.x: Major pivot (requires community discussion)

**Update Triggers**:
- Validation milestones reached
- Community consensus on expansion
- Founder vision evolution (with transparency)
- External context changes (legal, technical, social)

**Process**:
1. Propose changes via GitHub issue
2. 2-week community comment period
3. Maintainer decision (with rationale)
4. Document update + changelog

### Changelog

- **v1.0** (2026-02-09): Initial scope definition after OGP analysis

## Questions?

- **"Why so restrictive?"** → Focus wins. Validate small before going big.
- **"What about my use case?"** → Is it cannabis CEA <$1000? We got you. Otherwise, future possibility.
- **"Can I fork for [other crop]?"** → Absolutely! Open source means freedom to adapt.
- **"When will you support [X]?"** → When we hit expansion criteria. Help us validate core thesis faster.

## References

- [ARCHITECTURE.md](./ARCHITECTURE.md) - How we build within scope
- [CONTRIBUTING.md](./CONTRIBUTING.md) - How to contribute aligned work
- [README.md](./README.md) - Project overview
- [GitHub Issues](https://github.com/opengrowingalliance/.github/issues) - Scope discussion space
