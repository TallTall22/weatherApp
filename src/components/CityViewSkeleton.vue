<script setup>
  import AnimatedPlaceholder from "./AnimatedPlaceholder.vue";
</script>

<template>
  <div class="flex flex-col flex-1">
    <!-- 天氣狀況總覽 -->
    <div class="flex flex-col py-12 items-center">
      <AnimatedPlaceholder class="max-w-[300px] w-full mb-2" />
      <AnimatedPlaceholder class="max-w-[300px] w-full mb-12" />
      <AnimatedPlaceholder class="max-w-[300px] h-[100px] w-full mb-12" />
      <AnimatedPlaceholder class="max-w-[300px] w-full mb-8" />
      <AnimatedPlaceholder class="max-w-[300px] h-[75px] w-full" />
    </div>
    <!-- 每小時 -->
    <div class="flex flex-col py-12 px-8 items-center">
      <AnimatedPlaceholder class="max-w-screen-md h-[100px] w-full mb-12 " />
    </div>
    <!-- 一周 -->
    <div class="flex flex-col py-12 px-8 items-center">
      <AnimatedPlaceholder class="max-w-screen-md h-[100px] w-full mb-12" />
    </div>
  </div>
</template>
