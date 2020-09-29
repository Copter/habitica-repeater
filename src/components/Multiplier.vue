<template>
  <div class="sticky-top">
    <b-jumbotron style="padding-top: 16px; padding-bottom: 16px">
      <div>
        <div>Repeat the next button:</div>
        <div class="d-flex p-2">
          <b-form-input
            @change="changeMultiValue()"
            type="number"
            v-model="multiNumber"
            placeholder="1"
          ></b-form-input>
          <h2>×</h2>
        </div>
        <div style="margin-left: -24px; margin-right: -24px">
          <a
            href="https://habitica.fandom.com/wiki/User_blog:LadyAlys/Rate_Limiting_(Intentional_Slow-Downs)_in_Some_Third-Party_Tools"
            ><small>You can only send up to 30 requests per minute.</small></a
          >
        </div>
      </div>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    changedMultiNumber: Function,
  },
  data() {
    return {
      // Initial value
      multiNumber: "",
    };
  },
  methods: {
    changeMultiValue() {
      var multiplier = 1;
      if (this.multiNumber && this.multiNumber > 0)
        multiplier = this.multiNumber;
      if (this.multiNumber > 30) multiplier = 30;
      this.changedMultiNumber(multiplier);
    },

    resetField() {
      this.multiNumber = "";
    },
  },
};
</script>

<style>
.sticky-top {
  top: 30px;
}
@media (max-width: 767px) {
  .sticky-top {
    position: static;
  }
}
</style>