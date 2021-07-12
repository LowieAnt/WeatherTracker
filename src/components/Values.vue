<template>
    <div :key="value.id" v-for="value in values">
      <Value
          @delete-value="$emit('delete-value', value.id)"
          :value="value"
          :color="value.rainfall > 80 ? '#ff968f' : '#abd9af'"
      />
    </div>
</template>

<script>
import Value from "./Value";

export default {
  name: "Values",
  props: {
    values: Array,
    showViewAll: Boolean,
  },
  components: {
    Value,
  },
  emits: ["delete-value"],
};
</script>
