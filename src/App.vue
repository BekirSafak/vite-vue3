<template>
  <div class="flex h-screen place-items-center bg-gray-600">
    <div class="bg-cyan-50 text-emerald-200 w-4/12 text-center m-auto p-4 h-auto font-bold rounded-3xl">
      <h1 class="text-3xl text-emerald-400">{{ tittle }}</h1>
      <p class="mt-8">{{ counter }} adet su içtim</p>

      <div class="btnBox flex justify-center space-x-3">
        <button class="bg-blue-300 rounded-full py-2 px-4 font-bold text-teal-900 mt-10"
          @click="counter++">+1bardak</button>
        <button @click="toggleClick" class="bg-blue-200 rounded-full py-2 px-4 font-bold text-teal-900 mt-10">Hedefi
          Göster/Gizle</button>
      </div>

      <section v-if="show" class="mt-8">
        <p>Günlük bardak sayısı {{ dayGoals }} adet</p>
        <div class="total">{{ totalCup }}</div>
        <button class="bg-green-600 rounded-full py-2 px-4 font-bold text-teal-900 mt-2" @click="dayCup">Hedefi
          Artır</button>
      </section>

      <section class="targeter my-10">
        <h1 class="underline font-bold text-2xl text-emerald-400">Hedefine Ulaşanlar listesi</h1>
        <p class="my-3" v-for="user in users">{{ user.name }}</p>
      </section>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const tittle = "Kaç bardak su içtim?"
const counter = ref(0)
const show = ref(false)
const dayGoals = ref(10)
const users = ref([])

function toggleClick() {
  return show.value = !show.value
}

function dayCup(e) {
  let cup = dayGoals.value++
  if (cup >= 20) {
    alert(" Bu miktarda su böbreklere zarar verebilir.")
    dayGoals.value = 10;
  }
}

const totalCup = computed(() => {
  let totalWater = Number(dayGoals.value) - Number(counter.value)
  return "Hedefi tamamlamak için " + totalWater + " adet su içmeniz gerekli."
})



onMounted(() => {
  fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => response.json())
    .then(json => {
      console.log(json)
      users.value = json
    })
})

</script>