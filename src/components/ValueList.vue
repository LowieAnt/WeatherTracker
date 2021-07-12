<template>
  <div>
    <h2>Alle neerslag</h2>
    <Button
        @btn-click="$emit('toggle-view-all')"
        :text="showViewAll ? 'Close' : 'Show all values'"
        :color="'pink'"
    />
  </div>
</template>

<script>
import Button from "@/components/Button";

export default {
  name: "ValueList",
  props: {
    showViewAll: Boolean,
  },
  components: {
    Button,
  }
}
</script>

<style scoped>
div{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
h2{
  margin: 10px;
}
</style>
