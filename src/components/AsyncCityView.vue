<script setup>
  import axios from 'axios';
  import {useRoute} from "vue-router"

  const route=useRoute()
  const getWeatherData=async()=>{
    try{
      const weatherData=await axios.get(`https://api.openweathermap.org/data/3.0/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=6f2ba68781d8cfa0bc5f225762219f37`)

       // 計算現在日期及時間
      const localOffset = new Date().getTimezoneOffset() * 60000;
      const utc = weatherData.data.current.dt * 1000 + localOffset;
      weatherData.data.currentTime =utc + 1000 * weatherData.data.timezone_offset;

      // 計算每小時的天氣偏移量
      weatherData.data.hourly.forEach((hour) => {
        const utc = hour.dt * 1000 + localOffset;
        hour.currentTime =utc + 1000 * weatherData.data.timezone_offset;
    })

    return weatherData.data
    }catch(err){
      console.log(err)
    }
  }

  const weatherData=await getWeatherData()
</script>

<template>
  <div class="flex flex-col flex-1 items-center">
    <!--横幅-->
    <div v-if="route.query.preview" class="text-white p-4 bg-weather-secondary w-full text-center">
      <p>
        目前您正在預覽此城市，點擊「+」圖示開始追蹤此城市。
      </p>
    </div>
    <!-- 天氣狀況總覽-->
    <div class="flex flex-col items-center text-white py-12">
      <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
      <p class="text-sm mb-12">
        {{ 
          new Date(weatherData.currentTime).toLocaleDateString(
            "zh-TW",
            {
              weekday:"short",
              day:"2-digit",
              month:"long"
            }
          )
        }}
        {{ 
          new Date(weatherData.currentTime).toLocaleTimeString(
            "zh-TW",
            {
              timeStyle:"short"
            }
          )
        }}
      </p>
      <p class="text-8xl mb-8">
        {{ Math.round(weatherData.current.temp-273.15) }}&deg;
      </p>
      <div class="text-center">
        <p>
          體感溫度
          {{ Math.round(weatherData.current.feels_like-273.15)  }}
        </p>
        <p class="capitalize">
          {{ weatherData.current.weather[0].description }}
        </p>
        <img 
        class="w-[150px] h-auto" 
        :src="`http://openweathermap.org/img/wn/${weatherData.current.weather[0].icon}@2x.png`" 
        alt=""/>
      </div>
    </div>
    <hr class="border-white border-opacity-10 border w-full" />

    <!--每小時天氣-->
    <div class="max-w-screen-md w-full py-12">
      <div class="mx-8 text-white">
        <h2 class="mb-4">每小時天氣</h2>
        <div class="flex gap-10 overflow-x-scroll">
          <div v-for="hourData in weatherData.hourly" :key="hourData.dt" class="flex flex-col gap-4 items-center">
            <p class="whitespace-nowrap text-md">
              {{ 
                new Date(hourData.currentTime).toLocaleTimeString(
                  "zh-TW",{
                    hour:"numeric"
                  }
                )
              }}
            </p>
            <img
            class="w-auto h-[50px] object-cover"
             :src="`http://openweathermap.org/img/wn/${hourData.weather[0].icon}@2x.png`"
             alt=""
             />
             <p class="text-xl">
              {{ Math.round(hourData.temp-273.15) }}&deg;
             </p>
          </div>
        </div>
      </div>
    </div>
    <hr class="border-white border-opacity-10 border w-full" />
    <!--每周天氣-->
    <div class="max-w-screen-md w-full py-12">
      <div class="mx-8 text-white">
        <h2 class="mb-4">一周預期</h2>
        <div v-for="day in weatherData.daily"      :key="day.dt" class="flex items-center">
          <p class="flex-1">
            {{ 
              new Date(day.dt*1000).toLocaleDateString(
                "zh-TW",
                {
                  weekday:"long"
                }
              )  
            }}
          </p>
          <img
            class="w-[50px] h-[50px] object-cover"
             :src="`http://openweathermap.org/img/wn/${day.weather[0].icon}@2x.png`"
             alt=""
             />
             <div class="flex gap-2 flex-1 justify-end">
              <p>最高溫:{{ Math.round(day.temp.max-273.15) }}&deg;</p>
              <p>最低溫:{{ Math.round(day.temp.min-273.15) }}&deg;</p>
             </div>
        </div>
      </div>
    </div>
  </div>
</template>