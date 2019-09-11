# Vue Architecture Patterns

Here are a list of patterns most commonly used at Lumkani for our Vue codebases

## Inputs

### BaseInput.vue

Here we have a `q-input` field with all the attributes we commonly use in our forms 

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

Here is an example of how we would use `BaseInput.vue`

```vue
<template>
  <!-- New Way -->
  
  <base-input
    v-model="beneficiary.first_name"
    v-bind="{ 
      label: 'Beneficiary First Name', 
      ...passValidationProps('beneficiary.first_name') 
    }"
  />

  <!-- Old Way -->

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

<script>
export default {
  data() {
    return {
      beneficiary: {
        first_name: '',
      },
    };
  },
  validations: {
    beneficiary: {
      first_name: {
        required,
      },
    },
  },
};
</script>
```

`passValidationProps` is a method made available globally through a mixin. It helps us pass validation props to our input components