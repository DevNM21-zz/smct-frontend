<template>
  <div class="column li-i">
    {{ name }}
  </div>
</template>

<script>
export default {
  name: "ListItem",
  props: {
    name: String
  }
};
</script>

<style scoped></style>
