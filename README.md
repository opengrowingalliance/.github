# Open Growing Protocol (OGP)

**An open protocol for cannabis controlled environment agriculture (CEA), built on solarpunk principles.**

## What We're Building Now

The Open Growing Protocol is a standardized protocol for **cannabis cultivation automation**, starting with **home growers** using **affordable hardware** (target: <$1000). We're making precision growing techniques accessible to personal cultivators through:

- **Portable growing recipes** that work across different hardware setups
- **Progressive automation** that starts simple and scales with your needs
- **Hardware interoperability** that breaks vendor lock-in
- **Open knowledge sharing** with proper attribution for contributors

### Current Scope (v0.2-v2.0)

- **Crop**: Cannabis (photoperiod and autoflower cultivars)
- **Environment**: Controlled Environment Agriculture (CEA) - tents, closets, dedicated rooms
- **Users**: Home growers, small-scale cultivators
- **Hardware**: Raspberry Pi, ESP32, Arduino, and similar affordable platforms
- **Integration**: Common sensors (DHT22, soil moisture, pH), standard actuators (relays, pumps, lights)

**What this means**: You can define your growing recipe once—including light schedules, environmental targets, nutrient feeding—and deploy it on different hardware setups. The system handles the adaptation.

## Long-Term Vision (Aspirational)

While we're focused on cannabis CEA today, the protocol's design is guided by **solarpunk principles** that could enable broader applications:

- **Appropriate technology**: Solutions scaled to context, not needless complexity
- **Knowledge commons**: Growing knowledge belongs to communities, not corporations
- **Ecological integration**: Systems that work with natural processes
- **Decentralization**: Local autonomy without central control

These principles may eventually support expansion to other crops or contexts, but **that's not on the current roadmap**. We're building a solid foundation first.

## Why Cannabis CEA First?

This is an honest, strategic choice:

1. **Defined problem space**: Indoor cannabis growing has well-understood parameters and active community knowledge
2. **Motivated users**: Growers already invest in automation and actively share techniques
3. **Economic viability**: Even modest improvements in yield/quality create measurable value
4. **Regulatory clarity**: Legal markets provide legitimate use cases for documentation
5. **Proof of concept**: Success here validates the protocol before broader expansion

## Repository Structure

This monorepo provides unified access to specifications and schemas:

- `specifications/`: Protocol specifications (v1.0, v1.1, v2.0)
- `schemas/`: JSON schemas for devices and recipes
- `profile/`: Organization manifesto and community guidelines
- `memory-bank/`: Development context and architectural decisions

### Key Specifications

**Current focus**: v2.0 modular architecture

- [ogsp-root-spec.md](specifications/v2.0/ogsp-root-spec.md) - Protocol overview
- [core/ogsp-core-spec.md](specifications/v2.0/core/ogsp-core-spec.md) - Core architecture
- [hardware/ogsp-hardware-spec.md](specifications/v2.0/hardware/ogsp-hardware-spec.md) - Device abstraction
- [recipe/ogsp-recipe-spec.md](specifications/v2.0/recipe/ogsp-recipe-spec.md) - Growing recipes

**Legacy versions** (v1.0, v1.1) available for reference but not actively maintained.

### Schemas

- `device.schema.json` - Hardware device definitions (sensors, actuators, controllers)
- `recipe.schema.json` - Growing recipe structure and parameters

## Getting Started

**Note**: This is early-stage software. Expect rough edges and incomplete documentation.

1. Clone the repository:
   ```bash
   git clone https://github.com/opengrowingalliance/.github
   cd .github
   ```

2. Initialize submodules:
   ```bash
   git submodule init
   git submodule update
   ```

3. Read the [v2.0 root spec](specifications/v2.0/ogsp-root-spec.md) to understand the architecture

4. Explore example recipes (coming soon) or start defining your own

## Contributing

We welcome contributions that align with our current scope:

- Cannabis CEA recipe documentation
- Affordable hardware integrations
- Reference implementations for common platforms
- Documentation improvements
- Bug reports and feature requests

See [OGSP Contribution Guidelines](https://opengrowingalliance.solar/contribute) for details.

**What we're NOT looking for right now**:
- Integrations for non-cannabis crops (save these ideas for later!)
- Enterprise/commercial-scale features
- Cloud-dependent architectures

## Roadmap Honesty

**Current status**: Protocol specification in development, no production implementations yet.

**Near-term (2026 Q1-Q2)**:
- Finalize v2.0 spec based on cannabis CEA requirements
- Build reference implementation for Raspberry Pi
- Document 3-5 validated cannabis recipes
- Establish testing/validation process

**Medium-term (2026 Q3-Q4)**:
- Additional hardware platform support
- Community recipe library
- Integration with common cannabis grow equipment

**Long-term (aspirational)**:
- Protocol may prove adaptable to other crops
- May inform broader agricultural automation standards
- Could enable knowledge commons for diverse growing communities

We're intentionally **not** committing to specific timelines for expansion beyond cannabis CEA. We'll evaluate based on what we learn.

## License

Released under the [Apache 2.0 License](LICENSE).

---

*The Open Growing Protocol is guided by solarpunk values—technology that serves people and planet, knowledge in the commons, appropriate solutions over needless complexity. We're starting with cannabis CEA because it's a tractable problem that lets us prove these principles work. The future may hold more, but we're building it one grow at a time.*
