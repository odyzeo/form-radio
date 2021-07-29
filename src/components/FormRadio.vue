<template>
    <div
        :class="{
            'form-item--error': isErrors,
        }"
        class="form-item form-item--radio"
    >
        <label
            v-for="(option, key) in input.options"
            :key="`${input.name}_option_${key}`"
            :class="{
                [option.className]: option.className,
                'form-item--error': isErrors,
                'is-disabled': option.disabled,
                'is-readonly': option.readonly,
                'is-active': isActive(option),
            }"
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
                @click="click(option[field])"
                @change="change($event, option[field])"
            >
            <span class="form-radio__element"></span>

            <slot
                :option="option"
                name="option"
            >
                <span
                    class="form-radio__text"
                    v-text="translate(option.name)"
                ></span>
            </slot>
        </label>

        <form-errors
            v-if="showFormErrors"
            :form-errors="formErrors"
        ></form-errors>
        <form-errors
            v-else
            :form-errors="errors"
        ></form-errors>
    </div>
</template>

<script>
import { FormErrors, FormItem } from '@odyzeo/form-item';

export default {
    components: { FormErrors },
    extends: FormItem,
    props: {
        field: {
            type: String,
            default: 'value',
        },
    },
    data() {
        return {
            errors: [],
            localValue: '',
        };
    },
    computed: {
        isActive() {
            return (option) => this.localValue === option[this.field];
        },
        isEmpty() {
            return this.localValue == null || this.localValue === '';
        },
        isErrors() {
            return this.errors.length > 0;
        },
        isRequired() {
            return this.input.required || this.validator('required');
        },
    },
    watch: {
        value(n) {
            this.localValue = n;
        },
    },
    created() {
        this.init(this.value);
    },
    methods: {
        init(value) {
            this.localValue = value || '';
        },
        preventWhenReadonly(event, option) {
            if (option.readonly) {
                event.preventDefault();
            }
        },
        change(ev, value) {
            this.errors = [];
            this.validateByEventType(ev.type);

            this.$emit('input', value);
        },
        click(value) {
            this.$emit('click', value);
        },
        /**
         * Used by FormItem plugin
         */
        validate() {
            this.errors = [];

            if (this.isRequired && this.isEmpty) {
                this.errors.push(this.translate(this.requiredMessage));
            }
        },
    },
};
</script>

<style lang="less">
@import '../less/form-radio.less';
</style>
