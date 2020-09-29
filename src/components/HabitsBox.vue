<template>
  <div>
    <b-list-group
      v-for="habit in habits"
      :key="habit.text"
      style="text-align: left"
    >
      <b-list-group-item>
        <div class="row">
          <b-col style="max-width: 80px; padding-left: 13px; padding-right: 0">
            <b-button
              v-if="habit.up"
              @click="clickHabitBtn(habit.id, 'up', habit.text)"
              class="habit-score habit-up"
              variant="success"
              style="background-color: #24cc8f; border-color: #24cc8f"
            >
              <svg
                width="2em"
                height="2em"
                viewBox="0 0 16 16"
                class="bi bi-plus"
                fill="currentColor"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z"
                />
              </svg>
            </b-button>
            <b-button
              disabled
              v-if="!habit.up"
              class="habit-score habit-up"
              variant="secondary"
            >
              <svg
                width="2em"
                height="2em"
                viewBox="0 0 16 16"
                class="bi bi-plus"
                fill="currentColor"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z"
                />
              </svg>
            </b-button>
          </b-col>

          <b-col style="padding: 0">
            <span v-html="habit.text"></span>
          </b-col>

          <b-col style="max-width: 80px; padding-left: 0; padding-right: 13px">
            <b-button
              v-if="habit.down"
              @click="clickHabitBtn(habit.id, 'down', habit.text)"
              class="habit-score habit-down"
              variant="danger"
              style="background-color: #ff6165; border-color: #ff6165"
            >
              <svg
                width="2em"
                height="2em"
                viewBox="0 0 16 16"
                class="bi bi-dash"
                fill="currentColor"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  d="M4 8a.5.5 0 0 1 .5-.5h7a.5.5 0 0 1 0 1h-7A.5.5 0 0 1 4 8z"
                />
              </svg>
            </b-button>
            <b-button
              disabled
              v-if="!habit.down"
              class="habit-score habit-down"
              variant="secondary"
            >
              <svg
                width="2em"
                height="2em"
                viewBox="0 0 16 16"
                class="bi bi-dash"
                fill="currentColor"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  d="M4 8a.5.5 0 0 1 .5-.5h7a.5.5 0 0 1 0 1h-7A.5.5 0 0 1 4 8z"
                />
              </svg>
            </b-button>
          </b-col>
        </div>
      </b-list-group-item>
    </b-list-group>
  </div>
</template>

<script>
export default {
  props: {
    habits: Array,
    //scoreHabit: Function
    scoreLoopStart: Function,
  },
  /*data() {
      return {
        selected_id: null,
        selected_dir: null
      }
    },*/
  methods: {
    clickHabitBtn(id, dir, text) {
      //this.selected_id = id;
      //this.selected_dir = dir;
      //this.scoreHabit(id, dir)
      this.scoreLoopStart(id, dir, text);
    },
  },
};
</script>

<style scoped>
.habit-score {
  max-width: 46px;
  padding-left: 6px;
}
.habit-up {
  float: left;
}
.habit-down {
  float: right;
}

.column {
  float: left;
  padding: 10px;
}

.left,
.right {
  width: 80px;
}

.row:after {
  content: "";
  display: table;
  clear: both;
}
</style>
