<template>
  <div class="about">
    <h1>This is the blog page</h1>
    <button type="button" @click="handleButtonClick">CLICK ME!</button>
    <input type="text" v-model="input" />
    <button type="button" @click="handleInputClick">TRY SERVICE</button>
  </div>
</template>
<script lang="ts" setup>
import { ref } from 'vue'
const input = ref(null)
let server = ref(null)
const name = 'UP MOVE'
const uid = 'P4QcAOfEd2jEbIhJa7IstA=='
const MOVE_ID = '6658-4390-b53c-1de5e1453654'
// f7c9ba7e-6658-4390-b53c-1de5e1453654
const chars = [`f7c9b162-${MOVE_ID}`, `f7c9ba82-${MOVE_ID}`, `f7c9ba91-${MOVE_ID}`]
const handleButtonClick = async () => {
  const optionalServices = [
    'generic_access',
    'generic_attribute',
    'immediate_alert',
    'link_loss',
    'tx_power',
    'current_time',
    'reference_time_update',
    'next_dst_change',
    'glucose',
    'health_thermometer',
    'device_information',
    'heart_rate',
    'phone_alert_status',
    'battery_service',
    'blood_pressure',
    'alert_notification',
    'human_interface_device',
    'scan_parameters',
    'running_speed_and_cadence',
    'automation_io',
    'cycling_speed_and_cadence',
    'cycling_power',
    'location_and_navigation',
    'environmental_sensing',
    'body_composition',
    'user_data',
    'weight_scale',
    'bond_management',
    'continuous_glucose_monitoring',
    'internet_protocol_support',
    'indoor_positioning',
    'pulse_oximeter',
    'http_proxy',
    'transport_discovery',
    'object_transfer',
    'fitness_machine',
    'mesh_provisioning',
    'mesh_proxy',
    'reconnection_configuration',
    `f7c9ba7e-${MOVE_ID}`
  ]
  server.value = await navigator.bluetooth
    .requestDevice({ filters: [{ name }], optionalServices })
    .then((device) => device.gatt.connect())
}
const handleCharacteristicValueChanged = (event) => {
  const value = event.target.value
  console.log('Received ' + value)
}

const handleInputClick = async () => {
  if (server.value && input.value) {
    const availableService = await server.value.getPrimaryService(input.value)
    await availableService.getCharacteristic(chars[2]).then((char) => {
      console.log(char)
      char.addEventListener('characteristicvaluechanged', handleCharacteristicValueChanged)
    })
    // chars.forEach(async (c) => {
    //   await availableService
    //     .getCharacteristic((c) => c.startNotifications())
    //     .then((c) => {
    //       c.addEventListener('characteristicvaluechanged', handleCharacteristicValueChanged)
    //       console.log('Notifications have been started.', c)
    //     })
    // })
  }
}
</script>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
