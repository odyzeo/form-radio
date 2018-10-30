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
          :ref="radio.name"
          :input="radio"
        />
        <input
          class="btn-validate"
          type="submit"
          value="Show selected values">
      </form>
      <pre v-if="values">{{ values }}</pre>
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
      radio: {
        name: 'platform',
        required: true,
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
      values: null,
    };
  },
  methods: {
    submit() {
      const item = this.$refs[this.radio.name];
      if (item && item.validate) {
        item.validate();
        if (item.errors.length) return;
      }

      const formData = new FormData(this.$refs.form);
      this.values = Array.from(formData.entries()).reduce((memo, pair) => ({
        ...memo,
        [pair[0]]: pair[1],
      }), {});
    },
  },
};
</script>

<style lang="less">
@import '../src/less/app.less';
</style>
