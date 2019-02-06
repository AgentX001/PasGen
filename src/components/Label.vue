<template>
  <div class="label"><slot /></div>
</template>

<script>
export default {
  name: "Label",
};
</script>

<style scoped>
.label {
  margin-bottom: 16px;
  color: #cbcfd4;
  font-size: 10px;
  font-family: "Montserrat", sans-serif;
  text-transform: uppercase;
}
</style>
