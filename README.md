# @odyzeo/form-radio

Simple input radio Vue.js component.

## Installation

### npm

```
npm install --save @odyzeo/form-radio
```

### yarn

```
yarn add @odyzeo/form-radio
```

Import component in your where you want to use it and register it:

```
import FormRadio from '@odyzeo/form-radio';
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
    :form-errors="formErrors[radio.name]"
  ></form-radio>
</template>
```

```
<script>
import FormRadio from '@odyzeo/form-radio'

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
};
</script>
```

## Props

### input - required
| Property name | Type | Default value | Description |
| ------------- | ---- | ------------- | ----------- |
| `name` | string | | Input `name` attribute |
| `required` | boolean | `false` | If value is required |
| `options` | array | | Array of radio options [{ value: 'value', name: 'name' }] |
| `field` | string | `value` | Name of option property. |

### field {string} - optional
Name of option property. Default: `value`.

### value {string} - optional
This is the initial value of the form input radio.

### formErrors {array} - optional
Array of errors to display.

## Events
Component emits 'input' event with value of the element

## Development

```
npm run serve
```

or

```bash
yarn serve
```
