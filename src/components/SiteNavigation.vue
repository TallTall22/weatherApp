<script setup>
import {RouterLink} from "vue-router"
import BaseModal from "./BaseModal.vue";
import { ref } from "vue";

const modalActive=ref(null)
const toggleModal=()=>{
  modalActive.value=!modalActive.value
}
</script>

<template>
  <header class="sticky top-0 bg-weather-primary shadow-lg">
    <nav class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6">
      <RouterLink :to="{name:'home'}">
        <div class="flex items-center gap-3 flex-1">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p class="text-2xl">各地天氣</p>
        </div>
      </RouterLink>

      <div class="flex gap-3 flex-1 justify-end">
        <i class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer" @click="toggleModal"></i>
        <i class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer"></i>
      </div>

      <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
        <div class="text-black">
          <h1 class="text-2xl mb-1">關於：</h1>
          <p class="mb-4">
            各地天氣允許您追踪您選擇的城市的當前和未來天氣。
          </p>
          <h2 class="text-2xl">如何使用：</h2>
          <ol class="list-decimal list-inside mb-4">
            <li>
              透過在搜索欄中輸入城市名稱來搜索您的城市。
            </li>
            <li>
              從結果中選擇一個城市，這將帶您到所選城市的當前天氣。
            </li>
            <li>
              點擊右上角的 "+" 圖標跟蹤該城市。這將保存該城市以便在主頁上隨時查看。
            </li>
          </ol>

          <h2 class="text-2xl">刪除城市</h2>
          <p>
            如果您不再想追踪某個城市，只需在主頁中選擇該城市。在頁面底部將有一個刪除城市的選項。
          </p>

        </div>
      </BaseModal>
    </nav>
  </header>
</template>