# cryo-wiring-template

A template repository for dilution refrigerator wiring configuration data.

## Getting Started

### 1. Create your data repository

Click the **"Use this template"** button on GitHub to create your own repository.

### 2. Customize for your lab

#### Component catalog

Edit `components.yaml` to define the RF/microwave parts used in your lab:

```yaml
XMA-10dB:
  type: attenuator
  manufacturer: XMA
  model: XMA-2082-6431-10
  value_dB: 10.0

LNF-HEMT:
  type: amplifier
  manufacturer: LNF
  model: LNF-LNC03_14A
  amplifier_type: HEMT
  gain_dB: 40.0
```

#### Module templates

Edit files in `templates/` to define your standard wiring modules. These are the default component configurations applied to new cooldowns:

```yaml
# templates/control_module.yaml
control_standard:
  stages:
    50K:
    - XMA-10dB
    4K:
    - XMA-20dB
    MXC:
    - XMA-20dB
```

#### Metadata template

Edit `templates/metadata.yaml` to set the default metadata template. Placeholders (`cdNNN`, `YYYY-MM-DD`, `CRYO`) are replaced automatically when creating a new cooldown.

### 3. Remove sample data

Delete the `your-cryo/` directory — it exists only as an example:

```bash
rm -rf your-cryo/
git add -A && git commit -m "Remove sample data" && git push
```

### 4. Connect to cryo-wiring-app

Launch the [cryo-wiring-app](https://github.com/cryo-wiring/app) Web UI with your repository:

```bash
# Docker Compose (recommended)
REPO_URL=https://github.com/<your-user>/<your-repo>.git docker compose up

# Or with pip
pip install cryo-wiring-app
cryo-wiring-app --repo https://github.com/<your-user>/<your-repo>.git
```

Open http://localhost:3000 and start creating cooldowns from the Web UI.

### 5. (Optional) Use the CLI

```bash
pip install cryo-wiring-cli

cryo-wiring new anemone --chip chip.yaml
cryo-wiring build anemone/2026/cd001/
cryo-wiring validate anemone/2026/cd001/
```

## Repository Structure

```
<your-repo>/
├── .cryo-wiring.yaml            # Project configuration
├── components.yaml              # Component catalog (master)
├── templates/                   # Module templates (customizable)
│   ├── control_module.yaml
│   ├── readout_send_module.yaml
│   ├── readout_return_module.yaml
│   └── metadata.yaml
└── <cryo-name>/                 # Cryostat directory
    └── <YYYY>/                  # Year group
        ├── cd001/               # First cooldown
        │   ├── metadata.yaml    # Cooldown metadata
        │   ├── chip.yaml        # Chip information
        │   ├── components.yaml  # Component catalog (frozen copy)
        │   ├── control.yaml     # Control line wiring
        │   ├── readout_send.yaml
        │   ├── readout_return.yaml
        │   └── cooldown.yaml    # Resolved bundle
        └── cd002/               # Second cooldown
            └── ...
```

## Related Repositories

| Repository | Description |
|---|---|
| [cryo-wiring/spec](https://github.com/cryo-wiring/spec) | YAML format specification & schemas |
| [cryo-wiring/core](https://github.com/cryo-wiring/core) | Python library (models, validation, diagram, builder) |
| [cryo-wiring/cli](https://github.com/cryo-wiring/cli) | CLI tool |
| [cryo-wiring/app](https://github.com/cryo-wiring/app) | Web UI (FastAPI + Next.js) |
