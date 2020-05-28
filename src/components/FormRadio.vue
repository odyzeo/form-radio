<template>
    <div
        :class="{
            'form-item--error': isErrorClass,
        }"
        class="form-item"
    >
        <label
            v-for="(option, key) in input.options"
            :class="{
                'form-item--error': isErrorClass,
                'is-disabled': option.disabled,
                'is-readonly': option.readonly,
                'is-active': isActive(option),
            }"
            :key="`${input.name}_option_${key}`"
            class="form-radio"
            @click="preventWhenReadonly($event, option)"
        >
            <input
                ref="input"
                :value="option[field]"
                :name="input.name"
                :checked="isActive(option)"
                :disabled="option.disabled"
                :readonly="option.readonly"
                type="radio"
                class="form-radio__input"
                @change="change(option[field])"
            >
            <span class="form-radio__element"></span>

            <slot
                :option="option"
                name="option">
                <span class="form-radio__text">
                    {{ option.name }}
                </span>
            </slot>
        </label>
        <div
            v-for="(error, key) in errors"
            :key="`fe_error_${key}`"
            class="form-item__error"
            v-html="error"
        ></div>
        <div v-if="showFormErrors">
            <div
                v-for="(error, key) in formErrors"
                :key="`be_error_${key}`"
                class="form-item__error"
                v-html="error"
            ></div>
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
            type: [Array, Object],
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
            showFormErrors: (this.formErrors.length > 0),
        };
    },
    computed: {
        isErrorClass() {
            return this.errors.length || (this.formErrors.length && this.showFormErrors);
        },
        isActive() {
            return option => this.value === option[this.field];
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
                this.$el.scrollIntoView({
                    behavior: 'smooth',
                    block: 'nearest',
                });
            }
        },
        preventWhenReadonly(event, option) {
            if (option.readonly) {
                event.preventDefault();
            }
        },
        change(value) {
            this.showFormErrors = false;
            this.$emit('input', value);
        },
    },
};
</script>

<style lang="less">
@import '../less/form-radio.less';
</style>
