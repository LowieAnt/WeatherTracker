<template>
  <div
  :class="['value']"
  :style="{ background: color }">
    <h3>
      {{ value.month }} {{ value.year }}
      <i @click="$emit('delete-value', value.id)" class="fas fa-times"></i>
    </h3>
    <p>{{ value.rainfall }} liter</p>
  </div>
</template>

<script>
export default {
  name: "Value",
  props: {
    value: Object,
    color: String,
  },
};
</script>

<style scoped>
.fas {
  color: red;
}
.fas.fa-times {
  cursor: pointer;
}
.value {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
}
.value h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>

