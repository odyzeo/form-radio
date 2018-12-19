<template>
  <div
    id="app"
    class="app">
    <div class="container">
      <form
        ref="form"
        novalidate
        @submit.prevent="submit"
      >
        <h1>Which platform do you prefer?</h1>
        <form-radio
          :input="radio"
          v-model="radio.value"
          :form-errors="formErrors[radio.name]"
        ></form-radio>

        <p></p>

        {{radio.name}}: {{radio.value}}

        <p></p>

        <button class="btn-validate">
          Send
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import FormRadio from './components/FormRadio.vue';

export default {
  name: 'App',
  components: {
    FormRadio,
  },
  data() {
    return {
      formErrors: {},
      radio: {
        name: 'platform',
        required: true,
        value: 'android',
        options: [
          {
            value: 'ios',
            name: 'iOS',
          },
          {
            value: 'android',
            name: 'Android',
          },
          {
            value: 'windows',
            name: 'Windows Phone',
          },
        ],
      },
    };
  },
  methods: {
    submit() {
      this.formErrors = {
        platform: ['Are you sure?']
      }
    },
  },
};
</script>

<style lang="less">
@import '../src/less/app.less';
</style>
