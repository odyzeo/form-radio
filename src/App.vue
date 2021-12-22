<template>
    <div
        id="app"
        class="app"
    >
        <div class="container">
            <form
                ref="form"
                novalidate
                @submit.prevent="submit"
            >
                <h1>Which platform do you prefer?</h1>
                <form-radio
                    :key="className"
                    v-model="radio.value"
                    :class-name="className"
                    :input="radio"
                    :form-errors="formErrors[radio.name]"
                    :group-name="$options.GROUP_NAME"
                    @click="onClick"
                >
                    <template #option="{ option }">
                        <span class="form-radio__text">
                            {{ option.name }}
                        </span>
                    </template>
                </form-radio>

                <p>
                    {{ radio.name }}: {{ radio.value }}
                </p>
                <p>Previous value: {{ previousValue }}</p>

                <button class="btn-validate">
                    Send
                </button>

                <button
                    class="btn-validate"
                    @click.prevent="clear"
                >
                    Clear
                </button>

                <button
                    class="btn-validate"
                    @click.prevent="toggleClassName"
                >
                    Toggle className
                </button>
            </form>
        </div>
    </div>
</template>

<script>
import FormRadio from './components/FormRadio';

export default {
    GROUP_NAME: 'group-name',
    name: 'App',
    components: {
        FormRadio,
    },
    data() {
        return {
            className: null,
            formErrors: {},
            previousValue: null,
            radio: {
                name: 'platform',
                required: true,
                value: 'zeo',
                validatorEvent: 'onInput',
                options: [
                    {
                        value: 'ios',
                        name: 'iOS',
                        className: 'text-bold',
                    },
                    {
                        value: 'android',
                        name: 'Android',
                    },
                    {
                        value: 'windows',
                        name: 'Windows Phone',
                    },
                    {
                        value: 'ie',
                        name: 'IE',
                        disabled: true,
                    },
                    {
                        value: 'zeo',
                        name: 'Zeo',
                        readonly: true,
                    },
                ],
            },
        };
    },
    methods: {
        clear() {
            this.$formItem.clear(this.$options.GROUP_NAME);
        },
        onClick() {
            this.previousValue = this.radio.value;
        },
        submit() {
            this.formErrors = {
                platform: ['Are you sure?'],
            };
        },
        toggleClassName() {
            this.className = this.className === 'naked' ? null : 'naked';
        },
    },
};
</script>

<style lang="less">
@import '../src/less/app.less';
</style>
