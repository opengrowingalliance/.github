# Kubernetes Architecture Applicability to Cannabis CEA
**Senior Architect Evaluation**
**Date**: February 9, 2026
**Task**: #2 - Evaluate K8s control/data plane model for cannabis cultivation automation

---

## Executive Summary

**VERDICT: VALID ARCHITECTURAL PATTERN WITH CRITICAL CONSTRAINTS**

The Kubernetes control plane/data plane model **IS applicable** to cannabis CEA, but with important boundaries. This is **NOT just metaphor** - the architecture legitimately maps to biological control systems. However, the abstraction breaks at predictable points where physics/biology diverge from software orchestration.

**Viability Score: 7/10** - Architecturally sound for narrow scope, requires discipline to avoid scope creep into "agriculture Kubernetes."

---

## Critical Questions Answered

### 1. Is Kubernetes control/data plane a valid architectural model for CEA automation?

**YES - with bounded applicability**

#### Where the Model Holds

**Control Plane (Recipe Engine/Controller)**:
- **Declarative desired state**: Recipe defines target environment (temp: 24°C, humidity: 60%, PPFD: 400)
- **Reconciliation loop**: Controller continuously reads sensors, compares to desired state, issues actuator commands
- **State storage**: Historical data, current phase, execution logs (like etcd)
- **API server**: External control, monitoring, adjustments (like kube-apiserver)

**Data Plane (Hardware Layer)**:
- **Nodes**: Individual hardware modules (HVAC controller, LED driver, irrigation controller)
- **Workloads**: Environmental control tasks (maintain temp, deliver nutrients)
- **Resource management**: Power budgets, sensor polling rates, actuator duty cycles
- **Networking**: MQTT/HTTP communication between controller and hardware (like CNI)

**This mapping is LEGITIMATE**, not marketing fluff. Kubernetes invented none of these patterns - they're control theory fundamentals that work for both containers and greenhouses.

#### Where the Model Breaks

**Biological Systems ≠ Software Systems**:

1. **Time Constants**:
   - K8s: Pod restart in seconds
   - CEA: Temperature adjustment takes 15-60 minutes
   - **Implication**: Control loops must be MUCH slower, tuned per-parameter

2. **State Transitions**:
   - K8s: Deterministic (deployment → running → terminated)
   - CEA: Probabilistic (vegetative → flowering depends on genetics, not just photoperiod)
   - **Implication**: Phase transitions require confirmation sensors (visual inspection, trichome development), not just timers

3. **Error Recovery**:
   - K8s: Kill failed pod, start new one
   - CEA: Can't "restart" a stressed plant - must nurse it back
   - **Implication**: No "cattle not pets" philosophy - each plant is stateful

4. **Resource Fungibility**:
   - K8s: Any CPU/RAM works (within limits)
   - CEA: LED spectrum ≠ HPS light, even at "equivalent" PPFD
   - **Implication**: Hardware abstraction layer CANNOT treat "light source" as generic

5. **Coupling**:
   - K8s: Containers isolated
   - CEA: All parameters coupled (dehumidifier adds heat, lights add heat, irrigation affects humidity)
   - **Implication**: Controller needs MIMO (multiple-input-multiple-output) control, not independent loops

**Conclusion**: Model holds at architectural level (control loops, declarative state, hardware abstraction), breaks at implementation level (requires domain-specific control theory).

---

### 2. Where does the abstraction break (Kubernetes = software, CEA = physics)?

**Four Critical Abstraction Failures**:

#### A. Hardware Is Not Fungible

**K8s Assumption**: "Any node with sufficient CPU/RAM can run the workload"

**CEA Reality**:
- LED spectrum affects morphology (blue = compact, red = stretched)
- Substrate type affects water retention (rockwool ≠ coco coir)
- HVAC thermal coupling (dehumidifier heat ≠ neutral heating)

**Example Failure**:
```yaml
# This declarative recipe SEEMS portable:
environment:
  temperature:
    day: 24°C
  humidity: 60%
  light:
    PPFD: 400
    spectrum: "blue-dominant"
```

**But**:
- System A: LED + separate dehumidifier → Works
- System B: HPS + dehumidifier → Dehumidifier heat + HPS heat = 28°C overshoot
- System C: LED + air conditioner with dehumidification → Overcools to hit humidity target

**Fix**: Capability advertisement MUST include thermal coupling, control authority, response time:
```yaml
devices:
  - id: "led-1"
    capabilities:
      light:
        PPFD: {min: 0, max: 800}
        spectrum: {blue: 0.6, red: 0.3, far-red: 0.1}
      thermalCoupling:
        heatOutput: {watts: 150, affectedZones: ["air-temp"]}
        responseTime: {seconds: 300}
```

#### B. State Is Not Fungible

**K8s Assumption**: "Pods are cattle, not pets - kill and replace"

**CEA Reality**: Plants are **stateful pets**:
- Stress history affects future performance (heat stress → hermaphroditism risk)
- Genetics vary even within clones (epigenetic drift, somatic mutations)
- Root health non-visible but critical

**Example Failure**: Recipe says "transition to flowering when 30cm tall" but:
- Plant A (healthy): 30cm at 4 weeks, transitions successfully
- Plant B (early N deficiency): 30cm at 6 weeks, stunted root system → low yield

**Fix**: Success criteria MUST be multi-factor:
```yaml
phaseTransition:
  from: "vegetative"
  to: "flowering"
  criteria:
    AND:
      - height: {min: 30cm, max: 50cm}
      - healthScore: {min: 0.8}  # AI vision model
      - rootDevelopment: "adequate"  # requires root zone sensor
      - daysSinceLastStress: {min: 7}
```

#### C. Reconciliation Is Not Instantaneous

**K8s Assumption**: "Desired state → actual state in seconds/minutes"

**CEA Reality**:
- Temperature: 15-60 minutes
- Humidity: 10-30 minutes
- Substrate moisture: 1-6 hours
- Nutrient uptake: 12-72 hours
- Visible growth response: 3-14 days

**Example Failure**: Naive control loop (like K8s):
```
Every 10 seconds:
  IF temp < target: turn_on(heater)
  IF temp > target: turn_off(heater)
```

Result: Oscillation (overshoot → undershoot → repeat), plant stress

**Fix**: PID control with domain-specific tuning:
```python
# Cannabis CEA typical PID values
temp_controller = PIDController(
    Kp=5.0,   # Proportional gain
    Ki=0.1,   # Integral gain (slow accumulation)
    Td=300,   # Derivative time constant (5min smoothing)
    sample_time=60  # 1-minute loop (not 10 seconds)
)
```

#### D. Portability Is Probabilistic, Not Guaranteed

**K8s Assumption**: "YAML works anywhere with sufficient resources"

**CEA Reality**: Recipe outcome depends on:
- **Genetics**: "Strain X" is not a precise specification
  - Seed variability: 100 seeds = 100 phenotypes
  - Clone drift: "Same" mother plant changes over years
- **Local conditions**:
  - Water hardness (affects pH buffering, nutrient availability)
  - Substrate microbiome (location-specific, non-transferable)
  - Altitude (affects VPD calculations)

**Example**: Cannabis recipe tuned in Colorado (altitude 1600m):
- VPD calculated for 0.85 atm pressure
- Transplanted to sea level → VPD off by 15% → stress

**Fix**: Recipes are TEMPLATES requiring local calibration:
```yaml
recipe:
  metadata:
    portabilityLevel: "guided-template"  # NOT "guaranteed-execution"
  calibration:
    required:
      - waterQuality: {measure: ["hardness", "pH", "EC"]}
      - altitudeCorrection: {input: "meters-above-sea-level"}
      - geneticVerification: {method: "visual-comparison-to-reference-photos"}
    optional:
      - substrateTest: {microbiomeAnalysis: true}
```

---

### 3. Does declarative approach (desired state) work for biological systems?

**YES - but requires probabilistic interpretation**

#### What Works

**Declarative environmental parameters**:
```yaml
temperature:
  day: {optimal: 24, range: [20, 28]}
  night: {optimal: 20, range: [18, 24]}
```

This works because:
- Physics is deterministic (given working hardware)
- Feedback loops can stabilize
- Deviations detectable in real-time

#### What Doesn't Work

**Declarative biological outcomes**:
```yaml
# THIS IS FANTASY:
outputs:
  yield: 500g
  THC: 22%
  floweringTime: 56 days
```

Why it fails:
- **Genetics dominate**: Same recipe on different phenotypes = 30-50% yield variation
- **Accumulated history**: Early stress reduces final yield unpredictably
- **Emergent properties**: Terpene profile depends on gene expression, not just environment

#### Hybrid Approach: Declarative Inputs + Probabilistic Outputs

**What recipes SHOULD declare**:
```yaml
recipe:
  inputs:
    environment: {declarative: true}  # Temp, humidity, light (controllable)
    genetics: {specification: "fuzzy"}  # Strain name + reference photos (not precise)
  expectedOutputs:
    yield: {mean: 450g, stddev: 75g, confidence: 0.68}  # Not guaranteed
    floweringTime: {mode: 56days, range: [49, 63]}
    qualityProfile: {descriptive: true}  # Not quantitative guarantees
  validation:
    priorRuns: 12  # Sample size for statistics
    successRate: 0.83  # 10/12 runs met expectations
```

**Interpretation**: Recipes are "best practices with statistical backing," not "guaranteed programs."

---

### 4. Is recipe forking/sharing technically feasible?

**YES - with version control semantics, NOT app store semantics**

#### What Works (Git-Style Forking)

**Technical feasibility: HIGH**

```yaml
# Original recipe
recipe:
  id: "northern-lights-hydro-v2"
  author: "grower-alice"
  license: "CC-BY-SA-4.0"

# Fork with modifications
recipe:
  id: "northern-lights-hydro-colorado-altitude"
  forkedFrom: "northern-lights-hydro-v2"
  author: "grower-bob"
  modifications:
    - parameter: "VPD"
      reason: "Adjusted for 1600m altitude"
      change: {from: 1.0, to: 1.15}
    - parameter: "flowering-photoperiod"
      reason: "My phenotype responds better to 11/13"
      change: {from: "12/12", to: "11/13"}
```

**Why this works**:
- Clear provenance (like git commit history)
- Semantic versioning (breaking changes = major version)
- Diff-based modifications (what changed, why)
- Community review (like pull requests)

**GitHub-style recipe sharing IS feasible** with right data model.

#### What Doesn't Work (App Store-Style "Install & Run")

**Apple App Store semantics**:
- User: "Download recipe"
- System: "Running recipe..."
- User: "Why isn't it working?"

**Why it fails**:
- **No guaranteed compatibility**: Your hardware ≠ author's hardware
- **Genetics mismatch**: Your "Northern Lights" ≠ author's genetics
- **Local calibration required**: Water chemistry, altitude, etc.

**Fix**: Recipes require **onboarding workflow**:
```yaml
installation:
  steps:
    1. hardwareCompatibilityCheck:
        required: ["temperature-control", "humidity-control", "full-spectrum-light"]
        optional: ["CO2-injection", "automated-irrigation"]
    2. geneticConfirmation:
        prompt: "Does your plant match these reference photos?"
        photos: ["vegetative-week2.jpg", "early-flower.jpg"]
    3. localCalibration:
        waterTest: {pH: null, hardness: null, EC: null}
        altitude: null
    4. testRun:
        duration: "1-week"
        validation: "Confirm environmental parameters are stable"
    5. fullRun:
        monitoring: "Report deviations for recipe improvement"
```

**Conclusion**: Forking = YES, "plug-and-play" = NO

---

## Minimal Viable Architecture

### What Would Actually Work

**Scope: Single-strain cannabis CEA in 1-4 tent/room grows**

#### Control Plane Components

1. **Recipe Repository** (like OCI container registry):
   - Git-backed storage
   - Semantic versioning
   - Forking/diffing support
   - Digital signatures (prevent recipe poisoning)

2. **Controller** (like kubelet):
   - Runs on Raspberry Pi 4 or equivalent
   - Executes recipe phases
   - Reconciliation loop (1-minute interval)
   - Local data storage (time-series DB)

3. **API Server** (like kube-apiserver):
   - REST + WebSocket
   - Recipe upload/download
   - Real-time monitoring
   - Remote override (emergency control)

4. **Scheduler** (simplified vs K8s):
   - Phase transitions (germination → veg → flower)
   - Event scheduling (nutrient changes, defoliation reminders)
   - Calibration reminders

#### Data Plane Components

1. **Hardware Abstraction Layer**:
   - Device discovery (MQTT, mDNS)
   - Capability advertisement (not generic "sensor" - specific ranges, accuracy)
   - Standard command protocol

2. **Actuator Controllers** (like DaemonSets):
   - HVAC controller (temp/humidity)
   - Light controller (intensity, spectrum, photoperiod)
   - Irrigation controller (volume, frequency, EC/pH)
   - Air circulation controller

3. **Sensor Collectors**:
   - Environmental: temp, humidity, VPD, CO2
   - Substrate: moisture, EC, pH
   - Vision: cameras + AI health scoring

#### What's NOT in Minimal Architecture

- ❌ Multi-crop support (cannabis-only initially)
- ❌ Distributed growing (single-site only)
- ❌ Advanced AI (rule-based control sufficient)
- ❌ Commercial features (focus hobbyist/small grower)

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                     CONTROL PLANE                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌─────────────────┐  │
│  │   Recipe     │  │  Controller  │  │   API Server    │  │
│  │  Repository  │  │  (Pi 4)      │  │  (REST/WS)      │  │
│  │  (Git)       │  │              │  │                 │  │
│  │              │  │  - Reconcile │  │  - Monitoring   │  │
│  │  - Version   │  │  - Execute   │  │  - Control      │  │
│  │  - Fork      │  │  - Schedule  │  │  - Upload       │  │
│  │  - Diff      │  │              │  │                 │  │
│  └──────────────┘  └──────────────┘  └─────────────────┘  │
│                           │                                │
└───────────────────────────┼────────────────────────────────┘
                            │ Commands (MQTT)
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                      DATA PLANE                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────┐  ┌─────────────┐  ┌──────────────┐       │
│  │   HVAC      │  │   Lighting  │  │  Irrigation  │       │
│  │ Controller  │  │  Controller │  │  Controller  │       │
│  │             │  │             │  │              │       │
│  │ - Temp      │  │ - Intensity │  │ - Volume     │       │
│  │ - Humidity  │  │ - Spectrum  │  │ - EC/pH      │       │
│  │ - VPD       │  │ - Schedule  │  │ - Frequency  │       │
│  └─────────────┘  └─────────────┘  └──────────────┘       │
│         │                 │                 │              │
│         └─────────────────┴─────────────────┘              │
│                           │                                │
│                           ▼                                │
│  ┌────────────────────────────────────────────────────┐   │
│  │              Sensor Layer (MQTT)                   │   │
│  │  - Temp/RH/VPD  - Substrate  - Vision  - CO2      │   │
│  └────────────────────────────────────────────────────┘   │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

---

## Constraints & Boundaries

### Where K8s Model Applies

✅ **Declarative recipes**: Environmental targets as desired state
✅ **Reconciliation loops**: Continuous sensor → compare → actuate
✅ **Hardware abstraction**: Logical capabilities vs physical devices
✅ **Version control**: Recipe forking, diffing, semantic versioning
✅ **API-driven control**: External monitoring and override

### Where It Does NOT Apply

❌ **Automatic scaling**: Can't add more "plant pods" dynamically
❌ **Zero-downtime updates**: Changing photoperiod mid-flower = stress
❌ **Guaranteed portability**: Genetics + local conditions require calibration
❌ **Instant reconciliation**: Biology works on minutes-to-days timescales
❌ **Fungible resources**: LED ≠ HPS, rockwool ≠ soil

### Critical Dependencies

**For architecture to succeed**:

1. **Narrow genetic scope**:
   - Start with SINGLE strain
   - Expand to strain family (e.g., "Indica-dominant photoperiod")
   - Eventually: Multi-strain (requires per-strain calibration)

2. **Hardware standardization**:
   - Reference implementation BOM (<$500)
   - Certified compatible devices (tested interop)
   - Clear capability specs (not just "sensor")

3. **Calibration workflow**:
   - Mandatory local tuning (water, altitude, genetics confirmation)
   - Test runs before full grows
   - Feedback loop to improve recipes

4. **Community practices**:
   - Document modifications (why you forked)
   - Share results (success rate, yield data)
   - Peer review (catch overfitting to one setup)

---

## Risk Assessment

### Technical Risks

**HIGH RISK**:
- **Hardware coupling**: MIMO control (multiple inputs affect multiple outputs) is hard to tune generically
- **Sensor accuracy**: Cheap sensors drift → garbage in, garbage out
- **Actuator latency**: Slow response times require careful PID tuning

**MEDIUM RISK**:
- **Network reliability**: MQTT over WiFi in high-humidity environment
- **Power failures**: State recovery, avoid plant stress

**LOW RISK**:
- **Software architecture**: Control loops are well-understood
- **Data storage**: Time-series DBs are mature

### Biological Risks

**HIGH RISK**:
- **Genetic variability**: "Strain X" is not a precise specification → outcome variation
- **Cumulative stress**: Early mistakes compound into final yield loss
- **Disease/pests**: No sensor detects until visible (late stage)

**MEDIUM RISK**:
- **Phase transition failures**: Unclear triggers (is plant ready to flower?)
- **Nutrient lockout**: Complex chemistry, hard to debug

### Adoption Risks

**HIGH RISK**:
- **User expectation mismatch**: "Download recipe → guaranteed yield" is false
- **Setup complexity**: Calibration workflow may deter casual users
- **Hardware fragmentation**: If every vendor interprets spec differently

**MEDIUM RISK**:
- **Recipe quality**: No review process = bad recipes spread
- **Support burden**: Who answers "why didn't this work?"

---

## Recommendations

### DO Proceed With Architecture

**Reasons**:
1. **Control plane/data plane is sound** for CEA automation
2. **Declarative recipes work** for environmental parameters
3. **Git-style forking is feasible** with proper data model
4. **Scope is narrow enough** (cannabis CEA, not "all agriculture")

### Critical Constraints

1. **Market recipes as TEMPLATES, not PROGRAMS**:
   - Language: "Best practices based on 12 successful grows"
   - NOT: "Guaranteed 500g yield in 56 days"

2. **Mandatory calibration workflow**:
   - No "one-click install"
   - Onboarding: hardware check → genetics confirm → local tuning → test run

3. **Genetic scope limiting**:
   - Start: Single strain (e.g., "Northern Lights clone from MotherX")
   - Expand: Strain family (Indica-dominant photoperiod)
   - Avoid: "Works with any cannabis" (genetics too variable)

4. **Hardware certification program**:
   - Reference implementation (<$500 BOM)
   - Certified devices (tested compatibility)
   - Capability specs (ranges, accuracy, thermal coupling)

5. **Community quality control**:
   - Recipe peer review (like code review)
   - Success rate reporting (% of runs that met expectations)
   - Modification documentation (why you forked)

---

## Comparison to Validation Council Critiques

### Where This Evaluation Differs

**Council said**: "Recipe portability largely a myth"
**This eval**: **Agrees** - portability requires calibration, not plug-and-play

**Council said**: "Cannabis worst case for standardization"
**This eval**: **Partially agrees** - cannabis is hard, but CEA reduces variability vs field

**Council said**: "Impossible trinity: Universal + Autonomous + Portable"
**This eval**: **Agrees** - architecture trades off:
  - Universal → NO (cannabis CEA only)
  - Autonomous → PARTIAL (env control yes, plant care no)
  - Portable → QUALIFIED (with calibration workflow)

**Council said**: "Zero demand validation"
**This eval**: **Out of scope** - architecture is VIABLE if demand exists (separate question)

### Where Architecture Improves on Current OGP

**Current OGP v0.2 Issues**:
1. ❌ Claims universal applicability ("any crop")
2. ❌ Device schema too simple (7 capabilities, no accuracy/coupling)
3. ❌ Recipes imply guaranteed execution
4. ❌ No calibration workflow specified

**This Architecture**:
1. ✅ Scoped to cannabis CEA explicitly
2. ✅ Rich device capabilities (thermal coupling, response time, accuracy)
3. ✅ Recipes are probabilistic templates
4. ✅ Mandatory calibration + test runs

---

## Minimal Viable Product Specification

### MVP Scope

**Single-grow-tent automation for cannabis clone cultivation**

**Hardware**:
- 1x Raspberry Pi 4 (controller)
- 4x Temp/RH sensors (DHT22 or equivalent)
- 1x Full-spectrum LED (dimmable, 150-300W)
- 1x Inline fan + carbon filter
- 1x Space heater (smart plug controllable)
- 1x Dehumidifier (smart plug controllable)
- 1x Power relay board (8-channel)
- Optional: pH/EC sensors, camera

**BOM Target**: <$500 (excluding tent and nutrients)

### MVP Features

**Control Plane**:
- Single recipe execution (no multi-crop)
- 3 phases: vegetative → transition → flowering
- Manual phase transitions (require user confirmation)
- REST API for monitoring
- Local web UI (no cloud dependency)

**Data Plane**:
- Temperature control (±2°C)
- Humidity control (±5% RH)
- Light schedule (photoperiod timer)
- Data logging (1-minute intervals)

**Recipe Format**:
```yaml
recipe:
  name: "Northern Lights Clone - Tent v1"
  strain: "Northern Lights"
  propagation: "clone"
  phases:
    - name: "vegetative"
      duration: {weeks: 4}
      environment:
        temperature: {day: 24, night: 20, tolerance: 2}
        humidity: {target: 60, tolerance: 10}
        light: {hours: 18, intensity: "medium"}
    - name: "flowering"
      duration: {weeks: 8}
      environment:
        temperature: {day: 22, night: 18, tolerance: 2}
        humidity: {target: 50, tolerance: 10}
        light: {hours: 12, intensity: "high"}
```

### Success Criteria

**Technical**:
- ✅ Maintain temperature within ±2°C for 95% of time
- ✅ Maintain humidity within ±10% RH for 90% of time
- ✅ Execute photoperiod schedule with <5 min deviation
- ✅ Record 99.9% of sensor data (no significant gaps)

**User**:
- ✅ Setup in <4 hours (including calibration)
- ✅ Zero manual interventions for environment (nutrients manual initially)
- ✅ User can fork recipe and modify 1 parameter

**Biological**:
- ✅ Plant completes grow without major stress
- ✅ Yield within 20% of manual-grown baseline
- (NOT a target: specific yield/potency - too genetics-dependent)

---

## Conclusion

### Final Verdict: **PROCEED WITH CONSTRAINTS**

**The Kubernetes architecture IS valid for cannabis CEA**, but only if:

1. ✅ **Scope limited to CEA** (not field agriculture)
2. ✅ **Recipes are templates** requiring calibration (not plug-and-play programs)
3. ✅ **Hardware capabilities richly specified** (not generic "sensor" types)
4. ✅ **Genetics controlled** (clones > seeds, single strain initially)
5. ✅ **Calibration workflow mandatory** (onboarding, test runs, feedback)

### What Makes This Different from Failed Projects

**MIT OpenAg failed because**: Faked demos, no validation, universal scope
**This architecture**: Narrow scope (cannabis CEA), validation-first, honest capabilities

**Vertical farms failed because**: Capital intensity, thin margins, no tech advantage
**This architecture**: Low-cost reference (<$500), targets hobbyists (margin-tolerant), automation value clear

**IoT standards fragmented because**: Vendor lock-in incentives, broad scope
**This architecture**: Narrow domain, community-first (no vendors), open by design

### The Critical Test

**Can you build a working prototype for <$500 that outperforms manual growing?**

If **YES**: Architecture is viable, proceed to implementation
If **NO**: Architecture is academic exercise, abandon

**Recommendation**: Build MVP, validate with 3-5 users over 1-2 grow cycles BEFORE writing more specifications.

---

## Appendix: Kubernetes Concepts → CEA Mapping

| Kubernetes Concept | CEA Equivalent | Direct Analog? |
|--------------------|----------------|----------------|
| Pod | Growth phase | ✅ Yes - unit of execution |
| Deployment | Recipe | ✅ Yes - desired state declaration |
| Service | Sensor/actuator endpoint | ✅ Yes - stable API |
| Namespace | Grow room | ⚠️ Partial - isolation less strict |
| Node | Hardware controller | ✅ Yes - resource provider |
| kubelet | Local controller | ✅ Yes - reconciliation agent |
| etcd | Time-series DB | ⚠️ Partial - different query patterns |
| kube-apiserver | Recipe API | ✅ Yes - central control point |
| kube-scheduler | Phase scheduler | ⚠️ Partial - simpler (no bin packing) |
| ConfigMap | Recipe parameters | ✅ Yes - external configuration |
| Secret | Genetic lineage data | ⚠️ Partial - less security-critical |
| DaemonSet | Sensor collector | ✅ Yes - runs on every node |
| StatefulSet | N/A | ❌ No - plants aren't replicas |
| HPA (autoscaling) | N/A | ❌ No - can't scale plants |
| Rolling update | N/A | ❌ No - can't restart plants |
| CNI | MQTT/hardware bus | ⚠️ Partial - simpler topology |

**Legend**:
✅ Direct architectural analog
⚠️ Partial analog with caveats
❌ No analog / doesn't apply

---

**Evaluation Complete**
**Recommendation**: Architecture is sound for narrow scope. Validate with working prototype before expanding.
