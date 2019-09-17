## 0001 - BaseInput <Badge text="component" type="tip" />

The idea is to have a `BaseInput.vue` component that will abstract a lot of the duplication we have with our inputs

#### Example

```vue
<template>
  <base-input
    v-model="beneficiary.first_name"
    v-bind="{ label: 'Beneficiary First Name' }"
  />
</template>
```

* Author: Shailen Naidoo
* Proposal: [Check it out](/rfcs/0001-base-input)