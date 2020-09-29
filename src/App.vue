<template>
  <div id="app">
    <Header :apiLogin="apiLogin" :loginWarn="loginWarn" />
    <b-container class="bv-main-row">
      <b-row>
        <b-col class=".col" sm="3"> </b-col>
        <b-col sm="2">
          <Multiplier
            :changedMultiNumber="changedMultiNumber"
            ref="multiplierComponent"
          />
        </b-col>
        <b-col sm="4">
          <b-jumbotron v-if="!habits.length">
            <h4>Log In to see your Habits!</h4>
            <p class="text-secondary">
              If you have logged in and still see this, you probably don't have
              any habits yet...
            </p>
          </b-jumbotron>
          <HabitsBox
            v-if="habits.length"
            :habits="habits"
            :scoreLoopStart="scoreLoopStart"
          />
        </b-col>
      </b-row>
    </b-container>
    <footer class="footer">
      <div class="credits text-secondary">
        <a href="https://habitica.com/" style="color: inherit">Habitica</a>
        Third-Party Tool by
        <a
          href="https://habitica.com/profile/41ce497c-5344-4d4b-b1ed-0b5986619495"
          style="color: inherit"
          >Coal "Copter" Stone</a
        >
        <span style="float: right">
          <a
            href="https://github.com/Copter/habitica-repeater"
            style="color: inherit"
            >GitHub Repository</a
          >
        </span>
      </div>
    </footer>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import HabitsBox from "./components/HabitsBox.vue";
import Multiplier from "./components/Multiplier.vue";

export default {
  name: "App",
  components: {
    Header,
    HabitsBox,
    Multiplier,
  },
  props: {},
  data() {
    return {
      habits: [],
      multiplier: 1,
      hbUserId: "",
      hbApiToken: "",
      loginWarn: "",
    };
  },
  methods: {
    fetchApi(url, method) {
      return fetch(url, {
        method: method,
        headers: new Headers({
          "Content-Type": "application/json",
          "x-api-user": this.hbUserId,
          "x-api-key": this.hbApiToken,
          "x-client": "41ce497c-5344-4d4b-b1ed-0b5986619495-habitica-repeater",
        }),
      });
    },

    scoreHabit(id, dir) {
      return this.fetchApi(
        "https://habitica.com/api/v3/tasks/" + id + "/score/" + dir,
        "post"
      ).then((res) => {
        return res.json();
      });
    },

    changedMultiNumber(multiNumber) {
      this.multiplier = multiNumber;
    },

    scoreLoopStart(id, dir, text, loops = this.multiplier) {
      if (loops == 1) this.scoreHabit(id, dir);
      else this.scoreLoop(id, dir, text, loops);
      this.$refs.multiplierComponent.resetField();
    },

    scoreLoop(id, dir, text, loops = this.multiplier) {
      console.log(id, dir, text, loops);
      this.scoreHabit(id, dir).then(() => {
        setTimeout(() => {
          if (loops > 1) this.scoreLoop(id, dir, text, loops - 1), 500;
        });
      });
    },

    apiLogin(uid, apiToken) {
      this.hbUserId = uid;
      this.hbApiToken = apiToken;
      this.fetchApi("https://habitica.com/api/v3/tasks/user?type=habits", "get")
        .then((res) => {
          switch (res.status) {
            case 200:
              this.loginWarn = "";
              break;
            case 401:
              this.loginWarn = "Incorrect User ID or API Token";
              console.log(this.loginWarn);
              break;
            default:
              this.loginWarn = "Unknown Error. Is habitica online?";
              console.log(this.loginWarn);
              break;
          }
          return res.json();
        })
        .then((jsonData) => {
          this.habits = jsonData.data;
          var md = require("habitica-markdown");
          this.habits.forEach((habit) => {
            habit.text = md.render(habit.text);
          });
        });
    },
  },
  /*mounted: function() {},*/
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.bv-main-row {
  padding-top: 30px;
  padding-bottom: 80px;
  min-height: 100%;
  height: 100%;
  margin: 0 auto -50px;
}

html {
  position: relative;
  min-height: 100%;
}
body {
  /* Margin bottom by footer height */
  padding-bottom: 50px;
}
.footer {
  position: absolute;
  bottom: 0;
  width: 100%;
  /* Set the fixed height of the footer here */
  min-height: 50px;
  background-color: #f5f5f5;
  line-height: 50px;
  padding-left: 15 px;
  padding-right: 15 px;
}
.credits {
  text-align: left;
  padding-left: 16px;
  padding-right: 16px;
  font-size: 16px;
}
@media (max-width: 767px) {
  .credits {
    text-align: left;
    padding-left: 8px;
    padding-right: 8px;
    font-size: 9px;
  }
}
</style>
