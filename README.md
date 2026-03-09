# cryo-wiring-template

A template repository for dilution refrigerator wiring configuration data.

## Usage

### 1. Create a repository

Click the **"Use this template"** button on GitHub to create your own data repository.

### 2. Add fridge data

```bash
# Set up with the cryo-wiring CLI
pip install cryo-wiring-cli

# Create a new cooldown for a fridge
cryo-wiring new --fridge my-fridge --chip chip.yaml
```

Or use the [cryo-wiring-app](https://github.com/orangekame3/cryo-wiring-app) Web UI:

```bash
pip install cryo-wiring-cli-app

# Launch with this repository
cryo-wiring-app serve --repo https://github.com/<your-user>/<your-repo>.git
```

### 3. Structure

```
<your-repo>/
├── .cryo-wiring.yaml            # Project configuration
├── components.yaml              # Component catalog (master)
├── templates/                   # Module templates (customizable)
│   ├── control_module.yaml
│   ├── readout_send_module.yaml
│   ├── readout_return_module.yaml
│   └── metadata.yaml
└── your-cryo/                   # Fridge directory
    ├── current/                 # Working cooldown
    │   ├── metadata.yaml
    │   ├── chip.yaml
    │   ├── components.yaml
    │   ├── control.yaml
    │   ├── readout_send.yaml
    │   └── readout_return.yaml
    └── 2026-001/                # Snapshot (frozen past cooldown)
        ├── metadata.yaml
        ├── chip.yaml
        ├── components.yaml
        ├── control.yaml
        ├── readout_send.yaml
        └── readout_return.yaml
```

### 4. CI validation

Validation runs automatically when a PR is created. Errors are raised if wiring data violates the schema or contains inconsistencies.

## Customization

### Project configuration

The `.cryo-wiring.yaml` file at the repository root configures project-level settings:

```yaml
# Path to template files (modules, metadata template, etc.)
template_path: ./templates
```

When `template_path` is set, the CLI and app will use the templates in that directory instead of the built-in defaults. This lets you customize module definitions to match your lab's standard wiring configurations.

### Module templates

Edit files in the `templates/` directory to define your lab's standard wiring modules. For example, to add a filter to the standard control line template:

```yaml
# templates/control_module.yaml
control_standard:
  stages:
    50K:
    - XMA-10dB
    4K:
    - XMA-20dB
    - LPF-KL          # added lab-standard filter
    MXC:
    - XMA-20dB
```

### Component catalog

Define the components you use in `components.yaml`:

```yaml
XMA-10dB:
  type: attenuator
  model: XMA-2082-6431-10
  value_dB: 10.0

LNF-HEMT:
  type: amplifier
  model: LNF-LNC03_14A
  amplifier_type: HEMT
  gain_dB: 40.0
```

## Specification

For details on the data format, see [cryo-wiring-spec](https://github.com/orangekame3/cryo-wiring-spec).
