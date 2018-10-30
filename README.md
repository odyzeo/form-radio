# @odyzeo/form-radio

Simple input and textarea Vue.js component.

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
import 'FormRadio' from '@odyzeo/form-radio';
export default {
  components: { FormRadio },
}
```

Import styles or make your own.

```
import '@odyzeo/form-radio/dist/form-radio.css';
```

## Usage

```
<template>
  <h1>Which platform do you prefer?</h1>
  <form-radio
    :ref="radio.name"
    :input="radio"
    @input="selected = $event"
  />
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
      selected: '',
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
    };
  },
};
</script>
```

## Props

### input - required
| property name | type | description |
| --- | --- | --- |
| type | string | 'radio' |
| name | string | name attribute |
| required | boolean | if value is required |
| options | array | array of radio options [{ value: 'value', name: 'name' }] |

### field - optional
Name of option property, default is 'value'.

### value - optional
This is the initial value of the form radio.

### formErrors - optional
Use to display errors from your server/api.

## Events
Component emits 'input' event with value of the element

## CSS classes
*TODO*

## Development

```
npm run serve
```

or

```bash
yarn serve
```
