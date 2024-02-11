<script lang="js">

export default {

  mounted(){
    self = this
    document.addEventListener("keydown", function(event) {
      
      // if (["R","r"].includes(event.key)) {
      //   self.timerReset()
      // }
      
      if (event.key === " ") {
        if (!self.timerRunning){
          self.timerRun()
        }
        else {
          self.timerPause()
        }
      }
      if (event.key === "Backspace") {

        self.$swal.fire({
        title: '¿Deseas reiniciar el Timer?',
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
          // Timer.
          self.timerReset()
          self.$swal.fire('Reiniciado!', '', 'success')
        }
      })
      }

      if (!self.isReset){
        return
      }

      if (["Shift"].includes(event.key)) {
        self.starting_minutes++
      }
      if (["Control"].includes(event.key)) {
        if (self.starting_minutes > 0) {
          self.starting_minutes--
        }
      }
    });
  },
  data() {
    return {
      isReset: true,
      starting_minutes : 0,
      totalTime: 0,
      timerRunning: false,
      timerPaused: false,
      interval: null
    }
  },
  watch: {
    starting_minutes(newValue){
      this.totalTime= newValue * 60
    }
  },
  computed: {
    time: function () {
      return this.minutes + " : " + this.seconds;
    },
    minutes: function () {
      var min = Math.floor(this.totalTime / 60);
      return min >= 10 ? min : '0' + min;
    },
    seconds: function () {
      var sec = this.totalTime - (this.minutes * 60);
      return sec >= 10 ? sec : '0' + sec;
    }
  },
  methods: {
    timerRun() {
      this.isReset = false;
      this.timerRunning = true;
      this.interval = setInterval(this.countdownTimer, 1000);
      console.log(this.totalTime);
    },
    timerPause() {
      this.timerRunning = false;
      clearInterval(this.interval);
    },
    timerReset() {
      this.isReset = true;
      this.timerRunning = false;
      clearInterval(this.interval);
      this.totalTime = (this.starting_minutes * 60);
    },
    timerCountdown() {
      console.log('Working');
      this.timerRunning = true;
      this.interval = setInterval(this.updateCurrentTime, 1000);
      // Counts down from 60 seconds times 1000.
      setInterval(() => {
        this.timerMinutes--
      }, 60 * 1000)

      // Check if seconds at double zero and then make it a 59 to countdown from.
      // need another method of checking the number while in the loop and then adding a zero on the number under 10
      if (this.timerSeconds === '00') {
        this.timerSeconds = 59;
        setInterval(() => {
          this.timerSeconds--
        }, 1000);
      } else {
        setInterval(() => {
          this.timerSeconds--
        }, 1000);
      }
    },
    countdownTimer() {
      if (this.timerRunning == true) {
        if (this.totalTime == 0){
          this.timerReset()
          return
        }

        this.totalTime--;
      }
    }
  }
}

</script>

<template>
  <div class="text-center">
    <div class="center padding-4">
      <div id="timer2" v-html="time"></div>
    </div>
  </div>
</template>

<style lang="scss">


// new Vue({
//   el: "#app",
//   data: {

//   },
//   computed: {
//     time: function() {
//       return this.hours + ":" + this.minutes;
//     },
//     hours: function() {
//       var hrs = Math.floor((1000 / 60 / 60));
//       return hrs >= 10 ? hrs : '0' + hrs;
//     },
//     minutes: function() {
//       var min = Math.floor((1000 / 60) % 60);
//       return min >= 10 ? min : '0' + min;
//     }
//   }
// });
</style>
