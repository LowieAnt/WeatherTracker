<template>
  <div :key="value.id" v-for="value in values">
    <Value
      @delete-value="$emit('delete-value', value.id)"
      :value="value"
    />
  </div>
</template>

<script>
import Value from "./Value";

export default {
  name: "Values",
  props: {
    values: Array,
  },
  components: {
    Value,
  },
  emits: ["delete-value"],
};
</script>
