<template>
  <div class="flex flex-col h-screen max-h-screen">
    <!--  Search  & Results -->
    <div class="z-20 flex justify-center relative bg-hero-pattern bg-cover px-4 pt-8 pb-32">

      <!-- Search Input  -->
      <div class="w-full max-w-screen-sm">
        <h1 class="text-white text-center text-3xl pb-4">IP Address Tracker</h1>
        <div class="flex">
          <input v-model="queryIp" type="text"
            class="flex-1 py-3 px-2 rounded-tl-md rounded-bl-md focus:outline-none"
            placeholder="Search for any IP address or leave empty to get your ip info" />
            <i @click="getIPInfo" class="fas fa-chevron-right cursor-pointer bg-black text-white px-4 rounded-tr-md rounded-br-md flex items-center"></i>
        </div>
      </div>
       <!-- IP Info  -->
      <IPInfo v-if="ipInfo" :ipInfo='ipInfo'/>

    </div>

    <!-- MAP -->
    <div id="mapid" class="h-full z-10"></div>
  </div>
</template>

<script>
import IPInfo from '../components/IPInfo.vue'
import leaflet from 'leaflet'
import { onMounted, ref } from 'vue'
import axios from 'axios'
// @ is an alias to /src

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Home',
  components: { IPInfo },
  setup () {
    let mymap
    const queryIp = ref('')
    const ipInfo = ref(null)
    onMounted(() => {
      mymap = leaflet.map('mapid').setView([33.7490, -84.3880], 8)

      leaflet
        .tileLayer(
          'https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoiYWxleHNlIiwiYSI6ImNsM3E5ZjQweDEzZHMzZHJ4dXNleDJ2ZmYifQ.b7gVdbnokKQZQTXokzxZrQ',
          {
            attribution:
              'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
            maxZoom: 18,
            id: 'mapbox/streets-v11',
            tileSize: 512,
            zoomOffset: -1,
            accessToken:
              'pk.eyJ1IjoiYWxleHNlIiwiYSI6ImNsM3E5ZjQweDEzZHMzZHJ4dXNleDJ2ZmYifQ.b7gVdbnokKQZQTXokzxZrQ'
          }
        )
        .addTo(mymap)
    })
    const getIPInfo = async () => {
      try {
        const data = await axios.get(`https://geo.ipify.org/api/v2/country,city?apiKey=at_ikDYN2ep7vPsTeFs45XOjSoaSMo0H&ipAddress=${queryIp.value}`)
        const result = data.data
        ipInfo.value = {
          address: result.ip,
          state: result.location.region,
          timezone: result.location.timezone,
          isp: result.isp,
          lat: result.location.lat,
          lng: result.location.lng
        }
        console.log(ipInfo.value)
        leaflet.marker([ipInfo.value.lat, ipInfo.value.lng]).addTo(mymap)
        mymap.setView([ipInfo.value.lat, ipInfo.value.lng], 13)
      } catch (err) {
        alert(err.message)
      }
    }
    return { queryIp, ipInfo, getIPInfo }
  }
}
</script>
