<script setup>

import { inject, onMounted, ref } from 'vue';

import TimerComponent from './components/TimerComponentGabo.vue'

const swal = inject('$swal')

const titulo = ref("Puntuaciones")

const scoreboard = ref({
  team_one: {
    name: "Cabimas", fouls : 0 , score: 0, logo_source:"https://upload.wikimedia.org/wikipedia/commons/4/49/Escudo_Cabimas.PNG"
  },
  team_two:{
    name: "Ojeda", fouls : 0 , score: 0, logo_source:"https://upload.wikimedia.org/wikipedia/commons/4/49/Escudo_Cabimas.PNG"
  },
  periodo:0,
})

const pausar = () => {
  alert('Hola')
}

onMounted(()=>{
  document.addEventListener("keydown", function(event) {

    // SCORE Y PERIODOS
    if (["Q","q"].includes(event.key)) {
      scoreboard.value.team_one.score++;
    }
    if (["W","w"].includes(event.key)) {
      scoreboard.value.periodo++;
    }
    if (["E","e"].includes(event.key)) {
      scoreboard.value.team_two.score++;
    }
    if (["A","a"].includes(event.key)) {
      if (scoreboard.value.team_one.score>0){
        scoreboard.value.team_one.score--;
      }
    }
    if (["S","s"].includes(event.key)) {
      if (scoreboard.value.periodo>0){
        scoreboard.value.periodo--;
      }
    }
    if (["D","d"].includes(event.key)) {
      if (scoreboard.value.team_two.score>0){
        scoreboard.value.team_two.score--;
      }
    }

    // Fouls
    if (["U","u"].includes(event.key)) {
      scoreboard.value.team_one.fouls++;
    }
    if (["O","o"].includes(event.key)) {
      scoreboard.value.team_two.fouls++;
    }

    if (["J","j"].includes(event.key)) {
      if (scoreboard.value.team_one.fouls>0){
        scoreboard.value.team_one.fouls--;
      }
    }
    if (["L","l"].includes(event.key)) {
      if (scoreboard.value.team_two.fouls>0){
        scoreboard.value.team_two.fouls--;
      }
    }
    
    // Resetear
    if (["G","g"].includes(event.key)) {
      
      swal.fire({
        title: '¿Deseas reiniciar el Scoreboard?',
        showDenyButton: true,
        confirmButtonText: 'Si',
        denyButtonText: 'No',
        customClass: {
          actions: 'my-actions',
          confirmButton: 'order-2',
          denyButton: 'order-3',
        },
      }).then((result) => {
        if (result.isConfirmed) {
          // Score.
          scoreboard.value.team_one.score = 0;
          scoreboard.value.team_two.score = 0;
          
          // Fouls.
          scoreboard.value.team_one.fouls = 0;
          scoreboard.value.team_two.fouls = 0;
          
          // Periodos.
          scoreboard.value.periodo = 0;
          swal.fire('Reiniciado!', '', 'success')
        }
      })
      
    }

    // INTERFÁZ
    if (["T","t"].includes(event.key)) {
      const newValue = prompt("Inserte el título")
      if (newValue === "") {
        return
      }
      titulo.value = newValue
    }
    
  });
})

</script>



<template>
  <div class="w-full min-h-screen bg-black flex">
    <div class="flex flex-wrap max-w-7xl mx-auto my-auto">
      <div class="w-full pb-4">
        <p class="text-center uppercase text-white text-4xl lg:text-6xl font-bold">
          {{ titulo }}
        </p>
      </div>
      
      <div class="w-full flex pb-5">
        <div class="w-1/3 flex">
            <p class="text-4xl m-auto font-bold text-white"><img class="score_pics" :src="scoreboard.team_one.logo_source" alt=""></p>
        </div>
        <div class="w-1/3 mx-auto">
          <div class="score_timer">
            <!-- <vue-countdown :time="60 * 60 * 1000" v-slot="{ days, hours, minutes, seconds }">
              {{ hours }} : {{ minutes }} : {{ seconds }}
            </vue-countdown> -->
            <TimerComponent />
          </div>
        </div>
        <div class="w-1/3 flex">
            <p class="text-4xl m-auto font-bold text-white"><img class="score_pics" :src="scoreboard.team_two.logo_source" alt=""></p>
        </div>
      </div>


      <div class="flex flex-wrap w-full divide-x divide-zinc-300">
        <div class="w-1/3">
          <div class="w-full">
            <p class="score_name">{{ scoreboard.team_one.name }}</p>
            <p class="score_points">{{ scoreboard.team_one.score }}</p>
          </div>
          <div class="w-full">
            <p class="score_periodo_name">Faltas</p>
            <p class="score_falta">{{ scoreboard.team_one.fouls }}</p> 
          </div>
        </div>

        <div class="w-1/3 flex flex-wrap  items-center justify-center">
          
          <div class="w-full pb-4">
            <p class="score_periodo_name">Periodo</p>
            <p class="score_periodo">{{ scoreboard.periodo }}</p>
          </div>
        </div>

        <div class="w-1/3 flex flex-wrap items-center justify-center">
          <div class="w-full">
            <p class="score_name">{{ scoreboard.team_two.name }}</p>
            <p class="score_points">{{ scoreboard.team_two.score }}</p>
          </div>
          <div class="w-full">
            <p class="score_periodo_name">Faltas</p>
            <p class="score_falta">{{ scoreboard.team_two.fouls }}</p> 
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss">

@import url('https://fonts.googleapis.com/css2?family=Rubik+Mono+One&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Rock+Salt&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Honk&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Wallpoet&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Electrolize&display=swap');

body {
}

.score_timer {
  @apply py-6 mx-auto text-[#ffff00] text-center text-6xl my-5 md:w-[300px] border border-white;
  // font-family: 'Honk', system-ui;
  // font-family: 'Wallpoet', sans-serif;
  font-family: 'Electrolize', sans-serif;
}

.score_pics {
  @apply border-8 border-[#ffff00] rounded-lg lg:w-[150px] lg:h-[150px] w-[100px] h-[100px];
}

.score_name {
  @apply uppercase text-4xl text-center font-bold pb-2 text-white;
  // font-family: 'Rock Salt', cursive;
}

.score_periodo {
  @apply text-6xl text-center font-bold text-orange-400;  
  font-family: 'Electrolize', sans-serif;
}

.score_falta {
  @apply text-6xl text-center font-bold text-red-500;  
  font-family: 'Electrolize', sans-serif;
}

.score_periodo_name {
  @apply text-4xl text-center font-bold text-white;  
  font-family: 'Electrolize', sans-serif;
}

.score_points {
  @apply text-9xl text-center font-bold text-[#ffff00];
  font-family: 'Electrolize', sans-serif;
}

</style>
