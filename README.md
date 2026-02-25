# Project Solar Mining

## Overview
Project Solar Mining introduces a smart, self-regulating mining pool that enables households to mine Bitcoin using excess solar power.

<table>
  <tr>
    <td valign="top">
      <img src="images/dashboard_mining_pool_overview.png" alt="Mining Pool Overview">
    </td>
    <td valign="top">
      <img src="images/dashboard_mining_pool_individual_miners.png" alt="Individual Miners">
    </td>
  </tr>
</table>

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
### Extract
- Extract ProjectSolarMining_vx.x.x.zip 

The directory structure will look like this:
```
ProjectSolarMining_vx.x.x/
├── automations/
│   ├── mining_pool_emergency_shutdown.yaml
│   ├── mining_pool_regulate.yaml
│   ├── mining_pool_regulate_start.yaml
│   └── mining_pool_regulate_stop.yaml
├── dashboards/
│   └── dashboard-mining-pool.yaml
├── packages/
│   └── project-solar-mining/
│       ├── psm_miner_1.yaml
│       ├── psm_miner_2.yaml
│       ├── psm_miner_3.yaml
│       └── psm_mining_pool.yaml
├── license.txt
└── readme.txt
```

### Configuration
#### Package
- Create the directory structure /packages/project_solar_mining/ under /homeassistant/
- Copy the extracted files from /packages/project_solar_mining/ into /homeassistant/packages/project_solar_mining/
- In /homeassistant/configuration.yaml create the following section:
```yaml
homeassistant:
  packages: !include_dir_named packages
```

There should now be 3 psm_miner_x.yaml files in /homeassistant/packages/project_solar_mining/
That's one psm_miner_x.yaml file per NerdQAxe++ miner.

The first sensor in the psm_miner_1.yaml file looks like this:
```yaml
  - sensor:
      - name: "Miner 1"
        icon: "mdi:chip"
        state: "{{ states('input_boolean.miner_1_state') }}"
        attributes:
          api_endpoint_info: "http://192.168.0.10/api/system/info"
          api_endpoint_restart: "http://192.168.0.10/api/system/restart"
          api_endpoint_shutdown: "http://192.168.0.10/api/system/shutdown"
          power_need: 80
```
- In psm_miner_1.yaml:
	- Adjust the IP address in the api_endpoint_x attributes to your first miner's IP address
	- Set power_need to the maximum power the first miner can use
- In psm_miner_2.yaml:
	- Adjust the IP address in the api_endpoint_x attributes to your second miner's IP address
	- Set power_need to the maximum power the second miner can use
- In psm_miner_3.yaml:
	- Adjust the IP address in the api_endpoint_x attributes to your third miner's IP address
	- Set power_need to the maximum power the third miner can use

- In /homeassistant/packages/project_solar_mining/psm_mining_pool.yaml look for the following sensor
```yaml
  - sensor:
      - name: "Excess Solar Power"
        icon: mdi:solar-power-variant-outline
        state: "{{ states('sensor.dsmr_reading_electricity_currently_returned_watt') | int(0) }}"
        unit_of_measurement: "W"
        device_class: power
        state_class: measurement
```
- Change sensor.dsmr_reading_electricity_currently_returned_watt to your own's grid return sensor

#### Automations
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

#### Dashboard
- Create a new dashboard title Mining Pool, icon mdi:chip and url dashboard-mining-pool
- Edit the dashboard in Raw configuration editor
- Copy the content of dashboard-mining-pool.yaml over the placeholder yaml
- Save the dashboard

### Adding a miner
#### Package
To add e.g. a fourth miner:
- Copy /homeassistant/packages/project_solar_mining/psm_miner_3.yaml to /homeassistant/packages/project_solar_mining/psm_miner_4.yaml
- Replace all "Miner 3" instances by "Miner 4"
- Replace all "miner_3" instances by "miner_4"
- Adjust the IP address in the api_endpoint_x attributes to your fourth miner's IP address
- Set power_need to the maximum power the fourth miner can use

- In /homeassistant/packages/project_solar_mining/psm_mining_pool.yaml expand the following sensors to include the fourth miner:
	- "Mining Pool Online Miners"
	- "Mining Pool Power"
	- "Mining Pool Hashrate"
	- "Mining Pool Total Found Blocks"
	- "Mining Pool Best Diff Raw"

#### Automations
**MINING_POOL_EMERGENCY_SHUTDOWN**
- Add a condition to include "When Miner 4 VR Temperature is above 85"

**MINING_POOL_REGULATE**
- Copy block "Regulate Miner 3 with hysteresis" under this block
- Edit the new block in yaml:
	- Replace all "Miner 3" instances by "Miner 4"
	- Replace all "miner_3" instances by "miner_4"

**MINING_POOL_REGULATE_STOP**
- To "Input Boolean 'Turn off' [Miner 1 State] [Miner 2 State] [Miner 3 State]", add Miner 4 State

#### Dashboard
- Edit the dashboard in Raw configuration editor
- Copy Miner 3 yaml blocks under themselves and edit thme:
	- Replace all "Miner 3" instances by "Miner 4"
	- Replace all "miner_3" instances by "miner_4"