<script setup>
defineProps({
  city:{
    type:Object,
    default:()=>{}
  }
})
</script>

<template>
  <div class="flex py-6 px-3 bg-weather-secondary rounded-md shadow-md cursor-pointer">
    <div class="flex flex-col flex-1">
      <h2 class="text-3xl">{{ city.city }}</h2>
      <h3>{{ city.state }}</h3>
    </div>

    <div class="tlex flex-col gap-2">
      <p class="text-3xl slef-end">
        {{ Math.round((city.weather.main.temp-32)/1.8) }}&deg;
      </p>
      <div class="flex gap-2">
        <span class="text-xs">
          最高溫:{{ Math.round((city.weather.main.temp_max-32)/1.8) }}&deg;
        </span>
        <span class="text-xs">
          最低溫:{{ Math.round((city.weather.main.temp_min-32)/1.8) }}&deg;
        </span>
      </div>
    </div>
  </div>
</template>