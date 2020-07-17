<template>
  <v-app>
    <v-content>
      <v-container>
        <v-row>
          <v-col sm="6" offset-sm="3">
            <Pomodoro />
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import Pomodoro from "./components/Pomodoro";

export default {
  name: "App",

  components: {
    Pomodoro
  },

  data: () => ({})
};
</script>