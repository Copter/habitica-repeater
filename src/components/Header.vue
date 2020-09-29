<template>
  <div>
    <nav class="navbar navbar-dark justify-content-between">
      <a class="navbar-brand">Habitica Habit Repeater</a>
      <form class="form-inline">
        <div style="margin-right: 8px; padding-top: 8px">
          <h6 class="text-danger">{{ loginWarn }}</h6>
        </div>
        <input
          class="form-control"
          v-model="uid"
          type="text"
          placeholder="User ID"
        />
        <input
          class="form-control"
          v-model="apiToken"
          type="password"
          placeholder="API Token"
        />
        <b-button @click="clickLoginBtn(uid, apiToken)" class="btn btn-success">
          Login
        </b-button>
      </form>
    </nav>
  </div>
</template>

<script>
export default {
  props: {
    apiLogin: Function,
    loginWarn: String,
  },
  data() {
    return {
      uid: "",
      apiToken: "",
    };
  },
  methods: {
    clickLoginBtn(uid, apiToken) {
      this.apiLogin(uid, apiToken);
    },
  },
};
</script>

<style scoped>
nav {
  background-color: #6133b4;
}

input {
  margin-right: 8px;
}
</style>
