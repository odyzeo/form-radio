# @odyzeo/form-radio

Simple input radio Vue.js component.

<a href="https://form-radio-durmstrangd.vercel.app/" target="_blank">Demo</a>

## Installation

### npm

```
npm install --save @odyzeo/form-radio
```

### yarn

```
yarn add @odyzeo/form-radio
```

Import component in your where you want to use it and register it.
Remember to also use FormItem plugin.

```
import FormItem from '@odyzeo/form-item';
import FormRadio from '@odyzeo/form-radio';

Vue.use(FormItem);

export default {
    components: {
        FormRadio,
    },
}
```

Import styles or make your own.

```
import '@odyzeo/form-radio/dist/form-radio.css';
```

## Usage

```
<template>
    <form-radio
        :input="radio"
        v-model="radio.value"
    ></form-radio>
</template>
```

```
<script>
import FormRadio from './components/FormRadio';

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
                value: 'zeo',
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
};
</script>
```

## Props

Since `FormRadio` extends `FormItem` see [@odyzeo/form-item](https://www.npmjs.com/package/@odyzeo/form-item) for props.

Additional or overwritten props for `FormRadio`

### input - required
| Property name | Type | Default value | Description |
| ------------- | ---- | ------------- | ----------- |
| `name` | string | | Input `name` attribute |
| `required` | boolean | `false` | If value is required |
| `options` | array | | Array of radio options [{ value: 'value', name: 'name', disabled: false, className: 'custom-class-for-option' }] |

### field {string} - optional
Name of option property. Default: `value`.

## Slots
Using new slot syntax since 2.6.0+. [Check it out](https://vuejs.org/v2/guide/components-slots.html).

### option
```vue
<template #option="{ option }">
    <span class="form-radio__text">
        {{ option.name }}
    </span>
</template>
```

## Events
Component emits these events:
- `@input` - emits the value of the element
- `@click` - emits the value of clicked option (useful when you want to deselect option)

## Development

```
npm run serve
```
