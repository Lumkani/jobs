## BaseInput <Badge text="component" type="tip" />

The `BaseInput` component is aimed at reducing all the boilerplate of writing a standard `q-input` component for our forms

### What is the issue?

If you look through the current codebase there is a lot of duplication with our forms and form inputs. The below example showcases how some of our forms look. As you can see there are attributes that are repeatedly used with `q-input` 

```vue
<template>
  <form>
    <q-input
      v-model.lazy="beneficiary.first_name"
      float-label
      label="Beneficiary First Name"
      type="text"
      error-message="This field is required"
      :error="$v.beneficiary.first_name.$error"
      @blur="$v.beneficiary.first_name.$touch"
    />
    <q-input
      v-model.lazy="beneficiary.last_name"
      float-label
      label="Beneficiary Surname"
      type="text"
      error-message="This field is required"
      :error="$v.beneficiary.last_name.$error"
      @blur="$v.beneficiary.last_name.$touch"
    />
  </form>
</template>

<script>
export default {
  data() {
    return {
      beneficiary: {
        first_name: '',
        last_name: '',
      },
    };
  },
  validations: {
    beneficiary: {
      first_name: {
        required,
      },
      last_name: {
        required,
      },
    },
  },
};
</script>
```

### How can we solve this problem?

If we follow the [Atomic Design](http://bradfrost.com/blog/post/atomic-web-design/) methodology, we need to start isolating repeated use of components. This will reduce overhead of the pages and increase maintainability.

Here is the component created called `BaseInput` which abstracts a lot of the boilerplate into a single component

```vue
<template functional>
  <q-input
    :value="props.value"
    float-label
    :label="props.label"
    :type="props ? props.type : 'text'"
    :counter="props.counter ? props.counter : false"
    error-message="This field is required"
    :error="props.onError"
    @change="listeners.input($event.target.value)"
    @blur="props.onBlur"
  />
</template>
```

Here is the component in action

```vue
<template>
  <form>
    <base-input
      v-model="beneficiary.first_name"
      v-bind="{ 
        label: 'Beneficiary First Name', 
        ...passValidationProps('beneficiary.first_name') 
      }"
    />
    <base-input
      v-model="beneficiary.last_name"
      v-bind="{ 
        label: 'Beneficiary Surname', 
        ...passValidationProps('beneficiary.last_name') 
      }"
    />
  </form>
</template>

<script>
export default {
  data() {
    return {
      beneficiary: {
        first_name: '',
        last_name: '',
      },
    };
  },
  validations: {
    beneficiary: {
      first_name: {
        required,
      },
      last_name: {
        required,
      },
    },
  },
};
</script>
```

`passValidationProps` is a helper function that is made global via Vue Mixin, you can use `dot-notation` to reference validation logic.