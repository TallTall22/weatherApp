<script setup>
import CityCardskeleton from '@/components/CityCardskeleton.vue';
import CityList from '@/components/CityList.vue';
import axios from 'axios';
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router=useRouter()
const mapboxAPIKey=import.meta.env.VITE_MAPBOX_API_KEY
const searchQuery=ref("")
const queryTimeout=ref(null)
const mapboxSearchResult=ref(null)
const searchError=ref(null)

const getSearchResult=()=>{
  clearTimeout(queryTimeout.value)
  queryTimeout.value=setTimeout(async()=>{
        if(searchQuery.value!==""){
          try{
          const result=await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`)
          mapboxSearchResult.value=result.data.features
      }catch{
      searchError.value=true
    }
    return
    }
    mapboxSearchResult.value=null
  },300)
}

const previewCity=(searchResult)=>{
  const [city,state]=searchResult.place_name.split(",")
  let savedCity=null
  const cities=JSON.parse(localStorage.getItem("savedCities"))
  if(cities){
    savedCity=cities.some((theCity)=>theCity.city===city)
  }
  router.push({
    name:"cityView",
    params:{
      state:state===undefined?city:state.replace(" ",""),
      city:city
    },
    query:{
      lat:searchResult.geometry.coordinates[1],
      lng:searchResult.geometry.coordinates[0],
      ...savedCity?{}:{preview:true}
    }
  })  
}
</script>

<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input 
      type="text"
      v-model="searchQuery"
      @input="getSearchResult"
      placeholder="請輸入城市名"
      class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-sm"
      >
      <ul v-if="mapboxSearchResult" class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]">
        <p v-if="searchError">
          抱歉，發生錯誤，請重新輸入
        </p>
        <p v-if="!searchError && mapboxSearchResult.length===0">
          無匹配資料，請輸入其他城市名
        </p>
        <template v-else>
          <li v-for="searchResult in mapboxSearchResult" :key="searchResult.id" class="py-2 cursor-pointer"
          @click="previewCity(searchResult)">
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-4">
    <Suspense>
      <CityList />
      <template #fallback>
        <CityCardskeleton/>
      </template>
    </Suspense>
  </div>
  </main>
</template>
