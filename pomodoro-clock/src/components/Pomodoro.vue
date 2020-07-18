<template>
  <v-card class="promodoro-card">
    <v-tabs @change="changeCurrentTimer" v-model="currentTimer" color="black" grow>
      <v-tab v-for="timer in timers" :key="timer.name">{{ timer.name }}</v-tab>
    </v-tabs>

    <v-card class="pa-5 d-flex flex-column align-center timer-group" flat>
      <h1 class="time">{{ displayMinutes }}:{{ displaySeconds }}</h1>

      <div class="button-group">
        <v-btn @click="start" color="primary">
          <v-icon left small>mdi-play-circle-outline</v-icon>Start
        </v-btn>
        <v-btn @click="stop" color="error">
          <v-icon left small>mdi-stop-circle-outline</v-icon>Stop
        </v-btn>
        <v-btn @click="reset(timers[currentTimer].minutes)" :disabled="isRunning">
          <v-icon left small>mdi-restart</v-icon>Reset
        </v-btn>
      </div>
    </v-card>

    <SettingsDialog :dialog="dialog" :closeDialog="closeDialog" :save="save" :timers="timers" />
    <v-btn @click="dialog = true" class="fab" color="purple" dark medium absolute bottom left fab>
      <v-icon>mdi-cog-outline</v-icon>
    </v-btn>
  </v-card>
</template>

<script>
import SettingsDialog from "./SettingsDialog.vue";

export default {
  components: {
    SettingsDialog
  },
  data() {
    return {
      isRunning: false,
      timerInstance: null,
      totalSeconds: 25 * 60,
      currentTimer: 0,
      timers: [
        {
          name: "Pomodoro",
          minutes: 25
        },
        {
          name: "Short Break",
          minutes: 5
        },
        {
          name: "Long Break",
          minutes: 20
        }
      ],
      dialog: false
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
        if (this.totalSeconds > 0) {
          this.totalSeconds -= 1;
        }
      }, 1000);
    },
    stop() {
      this.isRunning = false;
      clearInterval(this.timerInstance);
    },
    reset(minutes) {
      this.stop();
      this.totalSeconds = minutes * 60;
    },
    changeCurrentTimer(num) {
      this.currentTimer = num;
      this.reset(this.timers[num].minutes);
    },
    closeDialog() {
      this.dialog = false;
    },
    save(updatedTimers) {
      this.timers = this.timers.map((timer, i) => {
        return { ...timer, minutes: parseInt(updatedTimers[i]) };
      });
      this.closeDialog();
    }
  }
};
</script>

<style lang="sass" scoped>
.v-card
  width: 800px
  height: 100%
  margin-top: 20%
.timer-group
  margin-top: 10px
.v-btn
  margin: 0 2px
.button-group
  position: absolute
  bottom: 0
  margin-bottom: 40px
.time
  position: relative
  margin-bottom: -80px
  font-size: 80px
  font-weight: 400
  text-align: center
.fab
  margin-bottom: -60px
</style>
