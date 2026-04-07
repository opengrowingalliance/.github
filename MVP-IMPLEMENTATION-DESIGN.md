# OGP Minimal Viable Implementation Design
## Cannabis CEA Automation - Personal Use Case

**Author**: Hardware Engineer (OGA Team)
**Date**: February 9, 2026
**Status**: Proposed Design
**Target User**: Founder's Personal Greenhouse

---

## Executive Summary

This document provides a **pragmatic, minimal viable implementation** of the Open Growing Protocol (OGP) for the founder's personal cannabis cultivation. The design prioritizes **real-world usability over architectural purity**, delivering a working system that can evolve from manual monitoring to full automation while embodying OGP's Kubernetes-inspired vision.

**Key Decisions**:
- **Hardware Platform**: Raspberry Pi 4 (4GB) for controller + ESP32s for sensors/actuators
- **Target BOM**: $600-800 for complete system (one 4x4 tent or small greenhouse)
- **Timeline**: 6-9 months to fully autonomous operation
- **First Success**: Monitoring + logging in 4-6 weeks

---

## 1. Hardware Stack

### 1.1 Core Controller

**Raspberry Pi 4 (4GB RAM)**
- **Why**: Sufficient compute for control loops, logging, web UI, and future AI models
- **Cost**: $55 (board only) / $95 (with case, power, SD card)
- **OS**: Raspberry Pi OS Lite (headless)
- **Connectivity**: WiFi + Ethernet for reliability

**Alternative**: Raspberry Pi 5 ($80) offers better performance but not required for MVP.

### 1.2 Sensor Network

#### Environmental Sensors (per zone)

| Sensor | Model | Measurement | Cost | Quantity | Total |
|--------|-------|-------------|------|----------|-------|
| **Temp/Humidity** | DHT22 or SHT31 | Air temp, RH | $10-15 | 2 | $20-30 |
| **CO2** | MH-Z19C | CO2 concentration | $25 | 1 | $25 |
| **Light (PAR)** | Apogee SQ-500 or DIY LUX→PAR | PPFD | $200 / $15 | 1 | $15-200 |
| **Substrate Moisture** | Capacitive soil sensor | Volumetric water content | $5 | 4 | $20 |
| **Substrate Temp** | DS18B20 | Root zone temperature | $3 | 4 | $12 |

**Note on PAR sensor**: The Apogee SQ-500 ($200) is the gold standard, but DIY LUX sensors with calibration curves work for MVP ($15 TSL2591 + calibration).

#### Water/Nutrient Sensors

| Sensor | Model | Measurement | Cost | Quantity | Total |
|--------|-------|-------------|------|----------|-------|
| **pH** | Atlas Scientific EZO-pH | Solution pH | $55 (kit) | 1 | $55 |
| **EC** | Atlas Scientific EZO-EC | Electrical conductivity | $55 (kit) | 1 | $55 |
| **Water Level** | Ultrasonic HC-SR04 | Reservoir level | $3 | 2 | $6 |
| **Flow Rate** | YF-S201 | Water flow | $5 | 1 | $5 |

**Total Sensor Cost**: $213-398 (depending on PAR sensor choice)

### 1.3 Actuator Network

#### Climate Control

| Actuator | Model/Type | Purpose | Cost | Quantity | Total |
|----------|-----------|---------|------|----------|-------|
| **Grow Light** | Mars Hydro TS1000 or Spider Farmer SF1000 | Primary lighting | $130-150 | 1-2 | $150-300 |
| **Light Dimmer** | PWM dimming driver (if not built-in) | Light intensity control | $25 | 1 | $25 |
| **Exhaust Fan** | AC Infinity CLOUDLINE T6 | Ventilation | $120 | 1 | $120 |
| **Circulation Fans** | 6" clip-on fans | Air movement | $15 | 2 | $30 |
| **Humidifier** | Evaporative or ultrasonic | Add humidity | $40 | 1 | $40 |
| **Dehumidifier** | Small compressor unit | Remove humidity | $180 | 1 | $180 |
| **Heater** | Ceramic space heater with relay control | Add heat | $25 | 1 | $25 |

#### Irrigation & Nutrients

| Actuator | Model/Type | Purpose | Cost | Quantity | Total |
|----------|-----------|---------|------|----------|-------|
| **Water Pump** | Submersible pump (12V DC) | Irrigation | $15 | 1 | $15 |
| **Nutrient Pumps** | Peristaltic dosing pumps | Precise nutrient dosing | $20 | 3 | $60 |
| **Solenoid Valves** | 12V DC normally closed | Irrigation zone control | $10 | 4 | $40 |
| **Air Pump** | Aquarium air pump | Nutrient solution aeration | $15 | 1 | $15 |

**Total Actuator Cost**: $700-900 (depending on light choice and climate needs)

### 1.4 Power & Relay Control

| Component | Purpose | Cost | Quantity | Total |
|-----------|---------|------|----------|-------|
| **8-Channel Relay Module (120V)** | Control high-power devices | $25 | 2 | $50 |
| **4-Channel Relay Module (12V)** | Control low-power devices | $10 | 1 | $10 |
| **24V Power Supply** | Power for pumps and valves | $20 | 1 | $20 |
| **12V Power Supply** | Power for sensors and ESP32s | $15 | 1 | $15 |

**Total Power/Control Cost**: $95

### 1.5 Communication & Integration

**ESP32 Microcontrollers** (for distributed I/O)
- **Purpose**: Offload sensor reading and actuator control from Pi
- **Communication**: MQTT over WiFi
- **Cost**: $10 each
- **Quantity**: 3-4 (climate zone, irrigation zone, nutrient system, backup)
- **Total**: $30-40

**Network Switch** (optional but recommended)
- **Purpose**: Wired Ethernet for Pi and critical devices
- **Cost**: $20-30

### 1.6 Physical Infrastructure

| Component | Purpose | Cost |
|-----------|---------|------|
| **Grow Tent** (4x4x7 ft) | Controlled environment | $100-200 |
| **Ducting & Fittings** | Ventilation routing | $40 |
| **Reservoir Tank** (20-30 gal) | Nutrient solution storage | $30-50 |
| **Irrigation Tubing** | Water distribution | $20 |
| **Drip Emitters** | Localized watering | $15 |
| **Mounting Hardware** | Sensor/actuator mounting | $30 |

**Total Infrastructure**: $235-355

---

## 2. Complete Bill of Materials (BOM)

| Category | Low Estimate | High Estimate |
|----------|-------------|---------------|
| Core Controller | $95 | $95 |
| Sensors | $213 | $398 |
| Actuators | $700 | $900 |
| Power & Control | $95 | $95 |
| ESP32 Network | $30 | $40 |
| Infrastructure | $235 | $355 |
| **TOTAL** | **$1,368** | **$1,883** |

**Cost Reduction Strategies for <$800 Target**:

1. **Start with Phase 1 only** (monitoring + manual control): ~$450
   - Sensors only, no actuators except what you already have
   - Log data, manually adjust environment
   - Validate recipe parameters before investing in automation

2. **DIY where possible**:
   - Use LUX sensors instead of PAR sensors (-$185)
   - Build relay modules from components (-$40)
   - Use existing fans/lights if compatible (-$200-400)

3. **Phased purchase**:
   - Phase 1: Controller + sensors ($450)
   - Phase 2: Basic automation (lights, fans) (+$250)
   - Phase 3: Advanced automation (irrigation, nutrients) (+$200)

**Realistic MVP Target**: $650-800 with phased approach

---

## 3. Software Architecture

### 3.1 Kubernetes-Inspired Control Model

The OGP controller follows Kubernetes patterns:

```
┌─────────────────────────────────────────────────┐
│              Recipe (Desired State)             │
│  - Declarative YAML defining plant needs        │
│  - Environmental parameters per growth phase     │
└─────────────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────┐
│            Controller (Reconciliation)          │
│  - Continuously monitors actual state           │
│  - Compares to desired state                    │
│  - Takes actions to reconcile differences       │
└─────────────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────┐
│          Hardware Abstraction Layer             │
│  - Sensor drivers (read actual state)           │
│  - Actuator drivers (change actual state)       │
│  - Plug-and-play device discovery               │
└─────────────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────┐
│              Physical Hardware                  │
│  - Sensors, actuators, grow environment         │
└─────────────────────────────────────────────────┘
```

### 3.2 Core Components

#### Recipe Engine
- **Input**: YAML recipe files (OGP Recipe Spec v2.2)
- **Function**: Parses recipe, validates schema, loads desired state for current phase
- **Language**: Python 3.11+
- **Libraries**: PyYAML, Pydantic (validation), jsonschema

#### State Manager
- **Function**: Maintains current state of all sensors and actuators
- **Storage**: InfluxDB (time-series) + SQLite (metadata, events)
- **Update Rate**: 10-60 seconds (configurable per sensor)

#### Reconciliation Controller
- **Function**: Core control loop
- **Algorithm**: PID controllers for continuous parameters (temp, humidity, CO2)
- **Cycle Time**: 60 seconds (adjustable based on system responsiveness)
- **Logic**:
  1. Read desired state from recipe (for current phase)
  2. Read actual state from sensors
  3. Calculate error (desired - actual)
  4. Apply control algorithm (PID)
  5. Send commands to actuators
  6. Log actions and state

#### Hardware Abstraction Layer (HAL)
- **Function**: Standardized interface to sensors/actuators
- **Protocol**: Plugin architecture (each device is a Python class)
- **Discovery**: Auto-detect devices on startup
- **Drivers**: Pre-built for common devices, custom drivers in `plugins/`

#### Communication Layer
- **Protocol**: MQTT (for ESP32 ↔ Pi communication)
- **Broker**: Mosquitto (running on Pi)
- **Topics**:
  - `ogp/sensors/{zone}/{sensor_type}` → sensor readings
  - `ogp/actuators/{zone}/{actuator_type}/command` → actuator commands
  - `ogp/actuators/{zone}/{actuator_type}/status` → actuator state

### 3.3 Technology Stack

| Component | Technology | Rationale |
|-----------|-----------|-----------|
| **Core Language** | Python 3.11+ | Rapid development, rich ecosystem for sensors/GPIO |
| **Web Framework** | FastAPI | Modern, async, auto-generated OpenAPI docs |
| **Frontend** | React + TypeScript | Reusable components, strong typing |
| **Time-Series DB** | InfluxDB 2.x | Purpose-built for sensor data |
| **Metadata DB** | SQLite | Lightweight, embedded, sufficient for MVP |
| **Message Broker** | Mosquitto MQTT | Standard for IoT, lightweight |
| **Task Queue** | Celery (optional) | For background tasks (data processing, AI inference) |
| **Logging** | Python logging → Loki or file | Centralized logging for debugging |

### 3.4 Control Loop Architecture

```python
# Pseudocode for reconciliation loop

while True:
    # 1. Determine current phase
    current_phase = recipe_engine.get_current_phase(start_time)

    # 2. Load desired state for this phase
    desired_state = current_phase.environment

    # 3. Read actual state from sensors
    actual_state = {
        'temperature': sensor_manager.read('temperature'),
        'humidity': sensor_manager.read('humidity'),
        'light_intensity': sensor_manager.read('par'),
        'co2': sensor_manager.read('co2'),
        # ... etc
    }

    # 4. Calculate control actions
    actions = []

    # Temperature control (PID)
    temp_error = desired_state['temperature']['optimal'] - actual_state['temperature']
    temp_control = pid_temperature.compute(temp_error)

    if temp_control > 0:  # Need heating
        actions.append(('heater', 'on'))
        actions.append(('exhaust_fan', 'low'))
    elif temp_control < 0:  # Need cooling
        actions.append(('heater', 'off'))
        actions.append(('exhaust_fan', 'high'))

    # Humidity control (PID)
    humidity_error = desired_state['humidity']['optimal'] - actual_state['humidity']
    humidity_control = pid_humidity.compute(humidity_error)

    if humidity_control > 0:  # Need more humidity
        actions.append(('humidifier', 'on'))
    elif humidity_control < 0:  # Need less humidity
        actions.append(('dehumidifier', 'on'))

    # Light control (schedule-based)
    current_hour = datetime.now().hour
    light_hours = desired_state['light']['dailyDuration']['hours']
    if lights_on_time <= current_hour < (lights_on_time + light_hours):
        target_intensity = desired_state['light']['intensity']['optimal']
        actions.append(('grow_light', 'on', {'intensity': target_intensity}))
    else:
        actions.append(('grow_light', 'off'))

    # 5. Execute actions
    for action in actions:
        actuator_manager.execute(action)

    # 6. Log state and actions
    influxdb.write({
        'measurement': 'environment',
        'time': datetime.now(),
        'fields': actual_state
    })
    influxdb.write({
        'measurement': 'control_actions',
        'time': datetime.now(),
        'fields': actions
    })

    # 7. Sleep until next cycle
    sleep(60)
```

---

## 4. Recipe Format (Simplified Example)

For the founder's personal use case, here's a minimal cannabis recipe:

```yaml
# cannabis-northern-lights-simple.yaml
version: "2.2.0"
metadata:
  id: "7f9c3a1e-4b2d-4c8f-9a6e-2d1f3b4c5d6e"
  name: "Northern Lights Auto - Simple Indoor"
  author: "founder@opengrowingalliance.solar"
  created: "2026-02-09T00:00:00Z"
  license: "CC-BY-SA-4.0"
  description: "Beginner-friendly autoflower recipe for 4x4 tent"

phases:
  - name: "Seedling"
    description: "First 2 weeks after germination"
    typicalDuration:
      value: 14
      unit: "days"
    environment:
      temperature:
        day: {optimal: 24, range: {min: 22, max: 26}}
        night: {optimal: 20, range: {min: 18, max: 22}}
      humidity:
        optimal: 65
        range: {min: 55, max: 75}
      light:
        dailyDuration: {hours: 18}
        intensity: {optimal: 300, range: {min: 200, max: 400}, unit: "PPFD"}
        spectrum: {primary: "blue"}
      substrate:
        moisture: {optimal: 60, range: {min: 50, max: 70}}
    nutrition:
      ec: {optimal: 0.6, range: {min: 0.4, max: 0.8}, unit: "mS/cm"}
      ph: {optimal: 6.0, range: {min: 5.8, max: 6.2}}
    successCriteria:
      primary: "3-4 sets of true leaves, healthy green color"

  - name: "Vegetative"
    description: "Rapid growth phase (weeks 3-5)"
    typicalDuration:
      value: 21
      unit: "days"
    environment:
      temperature:
        day: {optimal: 25, range: {min: 22, max: 28}}
        night: {optimal: 20, range: {min: 18, max: 23}}
      humidity:
        optimal: 60
        range: {min: 50, max: 70}
      light:
        dailyDuration: {hours: 18}
        intensity: {optimal: 500, range: {min: 400, max: 600}, unit: "PPFD"}
        spectrum: {primary: "blue", secondary: "red"}
      air:
        co2: {optimal: 800, range: {min: 600, max: 1000}, unit: "ppm"}
    nutrition:
      profile: "nitrogen-dominant"
      ec: {optimal: 1.2, range: {min: 1.0, max: 1.5}, unit: "mS/cm"}
      ph: {optimal: 6.0, range: {min: 5.8, max: 6.3}}
    successCriteria:
      primary: "Strong branching, 8-10 nodes, 30-40cm height"

  - name: "Flowering"
    description: "Flower development (weeks 6-12 for autoflower)"
    typicalDuration:
      value: 42
      unit: "days"
    environment:
      temperature:
        day: {optimal: 24, range: {min: 20, max: 26}}
        night: {optimal: 19, range: {min: 17, max: 21}}
      humidity:
        optimal: 45
        range: {min: 40, max: 55}
      light:
        dailyDuration: {hours: 18}  # Autoflower: keep 18/6
        intensity: {optimal: 600, range: {min: 500, max: 750}, unit: "PPFD"}
        spectrum: {primary: "red", secondary: "blue"}
      air:
        co2: {optimal: 1000, range: {min: 800, max: 1200}, unit: "ppm"}
    nutrition:
      profile: "phosphorus-potassium-dominant"
      ec: {optimal: 1.4, range: {min: 1.2, max: 1.8}, unit: "mS/cm"}
      ph: {optimal: 6.1, range: {min: 5.9, max: 6.4}}
    successCriteria:
      harvestIndicators: ["trichomes 70% milky, 30% amber", "pistils 80% brown"]

transitions:
  - from: "Seedling"
    to: "Vegetative"
    trigger:
      type: "time-based"
      condition: "after 14 days"
    environmentalShift:
      light:
        intensity: {from: 300, to: 500, transitionDays: 3}
      nutrition:
        ec: {from: 0.6, to: 1.2, transitionDays: 5}

  - from: "Vegetative"
    to: "Flowering"
    trigger:
      type: "automatic"
      condition: "autoflower genetics (age-triggered)"
    environmentalShift:
      humidity: {from: 60, to: 45, transitionDays: 7}
      nutrition:
        profile: {from: "nitrogen-dominant", to: "phosphorus-dominant"}

implementationGuidance:
  hardwareCapabilities:
    temperature: {controlRange: {min: 15, max: 32}, precision: 1.0}
    humidity: {controlRange: {min: 35, max: 75}, precision: 5.0}
    lighting: {spectrumControl: "recommended", intensityControl: "required"}
  adaptationGuidelines:
    noSpectrumControl: "Use full-spectrum white LED, focus on intensity and duration"
    limitedCO2: "Acceptable without CO2 enrichment, increase light intensity slightly"
```

---

## 5. MVP Implementation Phases

### Phase 1: Monitoring + Logging (Weeks 1-6)

**Goal**: Prove sensors work, collect baseline data, validate recipe parameters

**Hardware**:
- Raspberry Pi 4 + SD card
- DHT22 (temp/humidity) x2
- DIY LUX sensor (TSL2591)
- Capacitive soil moisture sensors x4
- DS18B20 (substrate temp) x4
- Basic relay module (for testing)

**Software**:
- Python sensor drivers
- InfluxDB setup
- Grafana dashboard (data visualization)
- Recipe engine (read-only)

**Deliverable**: Dashboard showing real-time environment data + recipe targets

**Success Metric**: 7 days of continuous data collection with <5% downtime

**Estimated Cost**: $450
**Estimated Time**: 4-6 weeks (includes learning curve)

### Phase 2: Basic Automation (Weeks 7-16)

**Goal**: Automate lighting schedule and basic climate control

**Hardware** (added to Phase 1):
- Grow light (Mars Hydro TS1000) with dimming
- Exhaust fan (AC Infinity CLOUDLINE T6)
- 8-channel relay module (120V)
- Optional: small humidifier/dehumidifier

**Software**:
- PID controllers (temperature, humidity)
- Light scheduler
- Actuator drivers
- Control loop (reconciliation)
- Alert system (email/SMS for out-of-range conditions)

**Deliverable**: System automatically maintains temperature ±2°C, humidity ±10%, lights on schedule

**Success Metric**: One complete grow cycle (12 weeks) with minimal manual intervention

**Estimated Cost**: +$400-500 (total ~$850-950)
**Estimated Time**: 6-10 weeks of development + 12 weeks of validation

### Phase 3: Advanced Automation (Weeks 17+)

**Goal**: Automate irrigation and nutrient dosing

**Hardware** (added to Phase 2):
- pH sensor + EC sensor (Atlas Scientific)
- Peristaltic dosing pumps x3
- Water pump + solenoid valves
- Reservoir tank with level sensors
- Air pump for solution aeration

**Software**:
- Irrigation controller (schedule + moisture-based)
- Nutrient dosing controller (pH/EC correction)
- Advanced PID tuning
- Recipe forking and experimentation tools
- Data export for recipe improvement

**Deliverable**: Fully autonomous system requiring only weekly reservoir refills

**Success Metric**: Two consecutive grow cycles with <5 hours manual intervention total

**Estimated Cost**: +$250-400 (total ~$1,100-1,350)
**Estimated Time**: 8-12 weeks

---

## 6. Technical Risks & Mitigations

### 6.1 Hardware Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| **Sensor failure mid-cycle** | Medium | High | Redundant critical sensors (2x temp/humidity), graceful degradation in software |
| **Relay failure (stuck on/off)** | Low | Critical | Use SSRs for critical loads, implement watchdog timers, physical kill switches |
| **WiFi connectivity loss** | Medium | Medium | Fail-safe mode: actuators revert to safe defaults (lights off, fans on), local logging |
| **Power outage** | Low | High | UPS for Pi + sensors (cheap $30 unit for 30min runtime), alert system |
| **pH/EC sensor drift** | High | Medium | Weekly calibration reminders, automatic anomaly detection, manual override |

### 6.2 Software Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| **Control loop bugs cause oscillation** | Medium | High | Extensive simulation before live deployment, PID auto-tuning, manual override always available |
| **Recipe parsing errors** | Low | Low | Strict schema validation, test suite with malformed recipes |
| **Database corruption** | Low | Medium | Daily backups to SD card + cloud (Dropbox/GDrive), SQLite WAL mode |
| **Concurrency issues** | Medium | Medium | Use asyncio properly, lock critical sections, avoid race conditions |

### 6.3 Operational Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| **Founder unavailable during critical issue** | Medium | High | Automated failsafes, remote monitoring/control via VPN, trusted friend backup |
| **Recipe doesn't match actual plant needs** | High | Medium | Start with proven community recipes, iterate slowly, manual monitoring initially |
| **System complexity overwhelms founder** | Medium | High | Simple web UI, good documentation, gradual feature rollout, remote support |

---

## 7. Development Timeline

### Months 1-2: Phase 1 (Monitoring)
- **Week 1-2**: Hardware assembly, sensor wiring, basic Pi setup
- **Week 3-4**: Sensor drivers, InfluxDB integration, Grafana dashboard
- **Week 5-6**: Recipe engine (read-only), validation with real sensors
- **Week 7-8**: Data collection, recipe parameter validation, tuning

### Months 3-4: Phase 2 (Basic Automation)
- **Week 9-10**: Actuator drivers (lights, fans), relay wiring
- **Week 11-12**: PID controller implementation, tuning
- **Week 13-14**: Control loop integration, testing
- **Week 15-16**: Alert system, web UI enhancements

### Months 5-9: Live Validation
- **Weeks 17-28**: Run complete grow cycle with Phase 2 automation
- **Goal**: Prove system stability, identify edge cases
- **Parallel work**: Begin Phase 3 design and part procurement

### Months 6-9: Phase 3 (Advanced Automation)
- **Week 29-32**: Irrigation system build, nutrient dosing hardware
- **Week 33-36**: pH/EC control software, irrigation scheduler
- **Week 37-40**: Integration testing, calibration procedures
- **Week 41+**: Long-term validation, recipe refinement

**Total Timeline**: 9-12 months to fully autonomous system

---

## 8. Cost Optimization Strategies

### For Hobbyist Budget (<$500)

1. **Use existing equipment**:
   - Repurpose grow lights (quantum board LEDs)
   - Use cheap analog timers for lights initially
   - Manual climate control (open/close vents)

2. **DIY sensors**:
   - Build PAR sensor from TSL2591 LUX sensor (-$185)
   - Use cheap DHT22 instead of SHT31 (-$5)
   - Skip CO2 sensor initially (-$25)
   - Use analog pH/EC meters for manual testing (-$110)

3. **Simplified automation**:
   - Phase 1 only: monitoring without actuators
   - Focus on data collection and learning
   - Gradually add automation as budget allows

**Minimum viable monitoring system**: $350-450

### For Serious Grower Budget ($1,000-1,500)

- Include all Phase 1-3 hardware from day one
- Buy premium sensors (Apogee PAR, Atlas Scientific pH/EC)
- Add CO2 supplementation system
- Include backup sensors and hot-swap components
- Professional-grade relays and wiring

---

## 9. Next Steps

### Immediate Actions (Week 1)

1. **Hardware Procurement**:
   - [ ] Order Raspberry Pi 4 kit
   - [ ] Order Phase 1 sensors
   - [ ] Order basic relay module
   - [ ] Order breadboard + jumper wires for prototyping

2. **Software Setup**:
   - [ ] Set up development environment (Python 3.11+, virtualenv)
   - [ ] Create GitHub repo: `remenoscodes.ogp-controller`
   - [ ] Write project README and architecture docs
   - [ ] Set up issue tracking with `git issue`

3. **Research & Learning**:
   - [ ] Read Raspberry Pi GPIO documentation
   - [ ] Study PID controller theory
   - [ ] Review OGP recipe specification v2.2
   - [ ] Survey existing open-source grow automation projects

### Week 2-4 Goals

1. **Sensor Integration**:
   - [ ] Wire DHT22 to Pi, read temp/humidity
   - [ ] Wire soil moisture sensors, validate readings
   - [ ] Set up InfluxDB, write sensor data
   - [ ] Create basic Grafana dashboard

2. **Recipe Engine**:
   - [ ] Implement YAML parser with Pydantic validation
   - [ ] Load Northern Lights recipe
   - [ ] Display current phase + desired parameters

### Month 2-3 Goals

1. **Actuator Control**:
   - [ ] Wire relay module, test with desk lamp
   - [ ] Implement light scheduler
   - [ ] Connect grow light, run 18/6 cycle
   - [ ] Add exhaust fan with speed control

2. **Control Loop**:
   - [ ] Implement basic PID controller for temperature
   - [ ] Tune PID parameters with test system
   - [ ] Add humidity control
   - [ ] Integrate with recipe engine

### Month 4-6 Goals

1. **Live Validation**:
   - [ ] Start first automated grow cycle
   - [ ] Monitor system 24/7, collect data
   - [ ] Fine-tune control parameters
   - [ ] Document issues and improvements

2. **Web UI**:
   - [ ] Build React dashboard (real-time monitoring)
   - [ ] Add manual override controls
   - [ ] Implement alert notifications
   - [ ] Mobile-responsive design

---

## 10. Success Criteria

### Phase 1 Success (Monitoring)
- [ ] 7 days of continuous sensor data with <5% downtime
- [ ] Accurate readings validated against manual measurements
- [ ] Recipe engine loads and displays current phase correctly
- [ ] Grafana dashboard shows live + historical data

### Phase 2 Success (Basic Automation)
- [ ] Complete one 12-week grow cycle
- [ ] Temperature maintained within ±2°C of target 95% of time
- [ ] Humidity maintained within ±10% of target 90% of time
- [ ] Lights run on schedule with 99.9% uptime
- [ ] <2 hours manual intervention per week

### Phase 3 Success (Advanced Automation)
- [ ] Two consecutive grow cycles
- [ ] Irrigation maintains substrate moisture in target range
- [ ] pH/EC maintained within target ranges 90% of time
- [ ] <5 hours manual intervention total over 24 weeks
- [ ] Harvest yield within 80% of strain potential

### Long-Term Success (Recipe Sharing)
- [ ] Publish refined recipe to GitHub
- [ ] Document lessons learned in recipe comments
- [ ] Fork recipe for different environment (e.g., outdoor greenhouse)
- [ ] Share data and results with OGA community

---

## 11. Conclusion

This design provides a **pragmatic path** from manual monitoring to full automation for the founder's personal cannabis cultivation. The phased approach allows for:

1. **Early validation**: Prove the concept with minimal investment
2. **Learning curve**: Build expertise gradually
3. **Cost management**: Spread investment over 9-12 months
4. **Risk mitigation**: Fail-safe defaults and manual overrides
5. **Future expansion**: Architecture supports additional features (AI, multi-zone, etc.)

**Key Philosophy**: Start simple, validate with real plants, iterate based on data. The system embodies OGP's Kubernetes-inspired vision (declarative recipes, reconciliation control) while remaining **accessible and practical** for a solo founder.

The founder should be able to:
- Monitor first grow cycle remotely (Phase 1) in 6-8 weeks
- Automate lights and climate (Phase 2) by month 4
- Achieve full automation (Phase 3) by month 9
- Share refined recipes with community (ongoing)

**Next step**: Order Phase 1 hardware and begin development. The system will pay for itself in improved yields and reduced labor within 2-3 grow cycles.

---

## Appendices

### A. Reference Implementations

1. **MudPi** (https://github.com/mudpi/mudpi-core)
   - Python-based automation for Raspberry Pi
   - Good sensor abstraction, MQTT support
   - Not recipe-based, but solid foundation

2. **Mycodo** (https://github.com/kizniche/Mycodo)
   - Full-featured environmental control
   - PID controllers, web UI, extensive sensor support
   - Heavyweight but feature-rich reference

3. **GrowBot** (https://github.com/j4ckofalltrades/growbot)
   - Simpler Arduino/ESP32 based system
   - Good for understanding control loops

### B. Sensor Calibration Procedures

(To be documented in separate operational guide)

### C. Recipe Validation Checklist

(To be documented in recipe authoring guide)

### D. Troubleshooting Guide

(To be developed during Phase 1-2 based on actual issues encountered)

---

**Document Version**: 1.0
**Last Updated**: February 9, 2026
**Author**: Hardware Engineer (OGA Agent Team)
**Review Status**: Pending team lead approval
**License**: CC-BY-SA-4.0
