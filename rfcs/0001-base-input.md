---
sidebar: false
---

- Author: Shailen Naidoo
- Start Date: 2019-09-14
- Implementation example link: [https://codepen.io/Naidoo/pen/jONpOxq?editors=1010](https://codepen.io/Naidoo/pen/jONpOxq?editors=1010)

# Summary

The idea is to have a `BaseInput.vue` component that will abstract a lot of the duplication we have with our inputs

# Basic example

#### New syntax

```vue
<template>
  <base-input
    v-model="beneficiary.first_name"
    v-bind="{ label: 'Beneficiary First Name' }"
  />
</template>
```

#### Current syntax

```vue
<template>
  <q-input
    v-model.lazy="beneficiary.first_name"
    float-label
    label="Beneficiary First Name"
    type="text"
    error-message="This field is required"
    :error="$v.beneficiary.first_name.$error"
    @blur="$v.beneficiary.first_name.$touch"
  />
</template>
```

# Motivation

Currently we have a bunch of the same `QInput` components repeated throughout our forms with loads of boilerplate, for example:

```vue{4,6,7,8,9}
<template>
  <q-input
    v-model.lazy="beneficiary.first_name"
    float-label
    label="Beneficiary First Name"
    type="text"
    error-message="This field is required"
    :error="$v.beneficiary.first_name.$error"
    @blur="$v.beneficiary.first_name.$touch"
  />
</template>
```

The highlighted lines is all the boilerplate we are writing, if we can reduce this we can:

* Reduce boilerplate
* Have a single component (`BaseInput`) to change instead of multiple instances of (`QInput`)
* No more need to think about passing the validators anymore

# Detailed design

The `BaseInput` component should be capable of:

* Proxying the props we give it
* Override default props
* Automatically add the `$error` and `$touch` props from Vuelidate where needed
* Bind event listeners depending on `counter` prop

We can use a Stateless Component so that we get less overhead with managing the internal state. Instead of using [Functional Template](https://alligator.io/vuejs/functional-components/), we would need to make use of [Render Functions](https://vuejs.org/v2/guide/render-function.html#Functional-Components).

Render Functions gives us access to properties that we cannot access normally. It's a much lower-level way of creating components and are perfect for creating really custom component logic.

Here is how we would implement the `BaseInput` component to do what we need!

```vue
<script>
const getValidators = ({ data, parent }) => {
  const { $error, $touch } = data.model.expression
    .split('.')
    .reduce((result, val) => result[val], parent.$v);

  return {
    onError: $error || false,
    onBlur: $touch || (() => {}),
  };
};

const getEvents = ({ props, listeners }) => {
  return props.counter ? {
    input: (e) => listeners.input(e),
  } : {
    change: (e) => listeners.input(e.target.value),
  };
};

export default (ctx) => {
  const { onError, onBlur } = getValidators(ctx);
  const events = getEvents(ctx);

  return (
    <QInput 
      props={{
        floatLabel: true,
        error: onError,
        errorMessage: ctx.props.errorMessage || 'This field is required',
        ...ctx.props,
      }} 
      on={{
        blur: onBlur,
        ...events,
      }} 
    />
  )
}
</script>
```

Here I am using [JSX](https://reactjs.org/docs/introducing-jsx.html) which is an XML-like syntax for JS.

# Drawbacks

* Overhead of updating `QInput` to `BaseInput`
* Using advanced Vue concepts in order to accomplish what we need
* The use of JSX might feel a bit weird

# Alternatives

N/A

# Adoption strategy

If we adopted this proposal it would dramatically reduce duplication and boilerplate. Since we have a single component being used throughout our forms it would be easier to update and extend it

# Unresolved questions

> Optional, but suggested for first drafts. What parts of the design are still TBD?