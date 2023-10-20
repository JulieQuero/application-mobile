<script setup>
import axios from 'axios';
import {onMounted,ref} from "vue";

let data = ref('');
let response = ref('');

onMounted(async () => {
  const response = await axios.post(
      'https://shelly-86-eu.shelly.cloud/device/status',
      {
        "id": "80646F827174",
        "auth_key": "MWRmYzM2dWlkE62C6C4C76F817CE0A3D2902F5B5D4C115E49B28CF8539114D9246505DE5D368D560D06020A92480"
      },
      {
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded',
        }
      }
  );
  data.value = response.data.data;
});

function onOff(){
  let newStatus = ref('');
  if (data.value.relays[0].ison === true){
    newStatus = false;
  }else{
    newStatus = true;
  }
  axios.post('http://192.168.1.100/actions', {
    "relay": 0,
    "on": newStatus
  })}
</script>

<template>
  <h1>Applications Mobiles</h1>
  <div v-if="data">
    <p>Name : {{data.device_status.wifi_sta.ssid}}</p><br />
    <p>IP : {{data.device_status.wifi_sta.ip}}</p><br />
    <p>Is On : {{data.device_status.relays[0].ison}}</p><br />
  </div>
  <button @onclick="onOff()">On / Off</button>
<!--  {{data}}-->
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

h1{
  text-align: center;
  margin-bottom: 20px;
}
.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
