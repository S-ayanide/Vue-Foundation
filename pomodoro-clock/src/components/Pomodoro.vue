<template>
  <v-card class="mt-10">
    <v-tabs @change="changeTimerType" v-model="timerType" color="black" grow>
      <v-tab v-for="tab in tabTitles" :key="tab">{{ tab }}</v-tab>

      <v-tabs-items v-model="timerType">
        <v-tab-item>
          <v-card color="basil" class="pa-5 d-flex flex-column align-center" flat>
            <h1 class="time">{{ displayMinutes }}:{{ displaySeconds }}</h1>

            <div class="button-group">
              <v-btn @click="start" color="primary">
                <v-icon left small>mdi-play-circle-outline</v-icon>Start
              </v-btn>
              <v-btn @click="stop" color="error">
                <v-icon left small>mdi-stop-circle-outline</v-icon>Stop
              </v-btn>
              <v-btn @click="reset" :disabled="isRunning">
                <v-icon left small>mdi-restart</v-icon>Reset
              </v-btn>
            </div>
          </v-card>
        </v-tab-item>
      </v-tabs-items>
    </v-tabs>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      isRunning: false,
      timerInstance: null,
      totalSeconds: 25 * 60,
      timerType: 0,
      tabTitles: ["Pomodoro", "Short Break", "Long Break"]
    };
  },
  computed: {
    displayMinutes() {
      const minutes = Math.floor(this.totalSeconds / 60);
      return this.formatTime(minutes);
    },
    displaySeconds() {
      const seconds = Math.floor(this.totalSeconds % 60);
      return this.formatTime(seconds);
    }
  },
  methods: {
    formatTime(time) {
      if (time < 10) {
        return "0" + time;
      }
      return time.toString();
    },
    start() {
      this.stop();
      this.isRunning = true;
      this.timerInstance = setInterval(() => {
        this.totalSeconds -= 1;
      }, 1000);
    },
    stop() {
      this.isRunning = false;
      clearInterval(this.timerInstance);
    },
    reset() {
      this.stop();
      this.totalSeconds = 25 * 60;
    },
    changeTimerType() {}
  }
};
</script>

<style lang="sass" scoped>
.v-card
    width: 800px
.v-btn
    margin: 0 2px
.time
    font-size: 80px
    font-weight: 400
    text-align: center
</style>