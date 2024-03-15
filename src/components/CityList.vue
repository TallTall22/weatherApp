<script setup>
import { ref } from 'vue';
import axios from 'axios'
import CityCard from './CityCard.vue';
import { useRouter } from 'vue-router';

const router=useRouter()

const savedCities=ref([])
const weatherAPIKey=import.meta.env.VITE_WEATHER_API_KEY
const getCities=async ()=>{
  if(localStorage.getItem('savedCities')){
    savedCities.value=JSON.parse(
      localStorage.getItem('savedCities')
    )

    const requests=[]
    savedCities.value.forEach((city)=>{
      requests.push(
        axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=${weatherAPIKey}&units=imperial`)
      )
    })

    const weatherData=await Promise.all(requests)

    weatherData.forEach((value,index)=>{
      savedCities.value[index].weather=value.data
    })
  }
}

await getCities()

const goToCityView=(city)=>{
  router.push({
    name:'cityView',
    params:{
      state:city.state,
      city:city.city
    },
    query:{
      lat:city.coords.lat,
      lng:city.coords.lng
    }
  })
}
</script>

<template>
  <div v-for="city in savedCities" :key="city.id">
    <CityCard :city="city" @click="goToCityView(city)"/>
  </div>

  <p v-if="savedCities.length===0">
    還沒有城市被加入清單，請在上方搜尋城市
  </p>
</template>
