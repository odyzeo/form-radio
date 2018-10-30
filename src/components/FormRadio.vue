<template>
  <div
    :class="{
      'form-item--error': errors.length || (formErrors.length && showFormErrors),
    }"
    class="form-item"
  >
    <div
      v-for="(option, key) in input.options"
      :key="`${input.name}_option_${key}`"
      class="form__radio"
    >
      <label class="radio">
        <input
          ref="input"
          :id="uid"
          :name="input.name"
          :value="option[field]"
          :checked="value == option[field]"
          v-model="localValue"
          type="radio"
          class="radio__input"
          @input="change(option[field])"
        >
        <span class="radio__element" />
        <span class="radio__text">
          {{ option.name }}
        </span>
      </label>
    </div>
    <div
      v-for="(error, key) in errors"
      :key="`fe_error_${key}`"
      class="form-item__error"
      v-html="error"
    />
    <div v-if="showFormErrors">
      <div
        v-for="(error, key) in formErrors"
        :key="`be_error_${key}`"
        class="form-item__error"
        v-html="error"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    input: {
      type: Object,
      required: true,
    },
    formErrors: {
      type: Array,
      default: () => [],
    },
    field: {
      type: String,
      default: 'value',
    },
    value: {
      type: [String, Number],
      default: null,
    },
  },
  data() {
    return {
      localValue: this.value || null,
      errors: [],
      showFormErrors: false,
    };
  },
  computed: {
    uid() {
      return `form-item-${this._uid}`;
    },
  },
  watch: {
    localValue() {
      this.validate();
    },
    value(o, n) {
      this.localValue = n;
      if (typeof o !== 'undefined' && n !== o) {
        this.showFormErrors = false;
      }
    },
    formErrors() {
      this.showFormErrors = true;
    },
  },
  methods: {
    validate(scroll = false) {
      this.errors = [];
      if (this.input.required && this.localValue === null) {
        this.errors.push('Please choose one option');
      }
      if (scroll && this.errors.length) {
        this.$el.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
      }
    },
    change(v) {
      this.showFormErrors = false;
      this.$emit('input', v);
    },
  },
};
</script>

<style lang="less">
@import '../less/radio.less';
</style>
