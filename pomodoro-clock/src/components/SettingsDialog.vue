<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline">Set Timer</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  v-for="(timer,i) in updatedTimers"
                  :key="i"
                  label="Time*"
                  v-model="updatedTimers[i]"
                  type="number"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
          <small>*indicates required field</small>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="closeDialog">Close</v-btn>
          <v-btn color="blue darken-1" text @click="save(updatedTimers)">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
export default {
  props: {
    dialog: {
      type: Boolean,
      require: true
    },
    closeDialog: {
      type: Function,
      require: true
    },
    timers: {
      type: Array,
      require: true
    },
    save: {
      type: Function,
      require: true
    }
  },
  data() {
    return {
      updatedTimers: []
    };
  },
  mounted() {
    this.updatedTimers = this.timers.map(timer => {
      return timer.minutes;
    });
  }
};
</script>
