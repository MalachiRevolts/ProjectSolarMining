# ProjectSolarMining

## Overview
Project Solar Mining introduces a smart, self-regulating mining pool that enables households to mine Bitcoin using excess solar power.

## Prerequisites
*Knowledge of each of these prerequisites is required and is not covered in this project.*

- Home Assistant (> v2026.2.1) with Zigbee setup
- Home Assistant integration "Meteorologisk institutt (Met.no)"
- HACS integration "Mushroom"
- Grid return sensor in Watt setup in Home Assistant (Used to evaluate excess solar power)
- Zigbee smart plug (e.g., Innr SP 240) setup in Home Assistant
- Powerstrip
- 3 NerdQAxe++ (> v1.0.36) miners (This project limits to 3 miners, but expansion is straightforward)

## Mining pool hardware setup
- Plug the NerdQAxe++ miners all in the same powerstrip
- Plug the zigbee smart plug in the wall socket
- Plug the powerstrip in the Innr Zigbee smart plug

## Home Assistant Project Solar Mining setup
### Package
- Create the directory structure /packages/project_solar_mining under /homeassistant/
- Copy the git files from /packages/project_solar_mining into /homeassistant/packages/project_solar_mining
- In /homeassistant/configuration.yaml create the following section:
```yaml
homeassistant:
  packages: !include_dir_named packages
```

### Automations
**MINING_POOL_EMERGENCY_SHUTDOWN**
- Create a new automation
- Edit the automation in YAML
- Copy the content of mining_pool_emergency_shutdown.yaml over the placeholder yaml
- Save the automation with the name MINING_POOL_EMERGENCY_SHUTDOWN

**MINING_POOL_REGULATE**
- Create a new automation
- Edit the automation in YAML
- Copy the content of mining_pool_regulate.yaml over the placeholder yaml
- Save the automation with the name MINING_POOL_REGULATE

**MINING_POOL_REGULATE_START**
- Create a new automation
- Edit the automation in YAML
- Copy the content of mining_pool_regulate_start.yaml over the placeholder yaml
- Save the automation with the name MINING_POOL_REGULATE_START

**MINING_POOL_REGULATE_STOP**
- Create a new automation
- Edit the automation in YAML
- Copy the content of mining_pool_regulate_stop.yaml over the placeholder yaml
- Save the automation with the name MINING_POOL_REGULATE_STOP

### Dashboard
- Create a new dashboard title Mining Pool, icon mdi:chip and url dashboard-mining-pool
- Edit the dashboard in Raw configuration editor
- Copy the content of dashboard-mining-pool.yaml over the placeholder yaml
- Save the dashboard