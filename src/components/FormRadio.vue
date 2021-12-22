<template>
    <div :class="componentClassName">
        <label
            v-for="(option, key) in input.options"
            :key="`${input.name}_option_${key}`"
            :class="getOptionClass(option)"
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
                :class="$options.CLASS_NAME.input"
                @click="click(option[field])"
                @change="change($event, option[field])"
            >
            <span :class="$options.CLASS_NAME.element"></span>

            <slot
                :option="option"
                name="option"
            >
                <span
                    :class="$options.CLASS_NAME.text"
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
        componentClassName() {
            return {
                [this.getClassName()]: true,
                [this.getClassName(null, 'error')]: this.isErrors,
            };
        },
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
        this.initClassName();
    },
    methods: {
        init(value) {
            this.localValue = value || '';
        },
        initClassName() {
            this.$options.CLASS_NAME = {
                element: this.getClassName('element'),
                input: this.getClassName('input'),
                label: this.getClassName('label'),
                text: this.getClassName('text'),
            };
        },
        getClassName(element = null, modifier = null) {
            let className = this.className ?? 'form-radio';
            if (element) {
                className = `${className}__${element}`;
            }
            if (modifier) {
                className = `${className}--${modifier}`;
            }
            return className;
        },
        getOptionClass(option) {
            return {
                [option.className]: option.className,
                [this.getClassName('label')]: true,
                [this.getClassName('label', 'error')]: this.isErrors,
                [this.getClassName('label', 'disabled')]: option.disabled,
                [this.getClassName('label', 'readonly')]: option.readonly,
                [this.getClassName('label', 'active')]: this.isActive(option),
            };
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
