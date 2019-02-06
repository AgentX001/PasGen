<template>
  <div class="checkbox">
    <div class="checkbox__container">
      <input type="checkbox" v-bind:checked="value" v-on:input="onInput">
      <span class="checkbox__slider" @click="toggle"></span>
    </div>
    <label>{{ label }}</label>
  </div>
</template>

<script>
export default {
  props: ["value", "label"],
  methods: {
    toggle() {
      this.$emit("input", !this.value);
    },
    onInput(event) {
      this.$emit("input", event.target.value);
    }
  }
};
</script>


<style lang="scss" scoped>
@import "../vars";

$slider-size: 20px;

.checkbox {
  width: fit-content;
}

.checkbox__container {
  position: relative;
  display: inline-block;
  width: $slider-size * 2;
  height: $slider-size;

  & input {
    opacity: 0;
    width: 0;
    height: 0;
  }
}

.checkbox__slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  border-radius: $slider-size/2;
  background-color: #747d88;
  transition: 0.2s;

  &:before {
    position: absolute;
    content: "";
    height: $slider-size;
    width: $slider-size;
    background: white;
    border-radius: 50%;
    transition: all 0.4s;
  }
}

input:checked + .checkbox__slider {
  background-color: #ffffff;
}

input:checked + .checkbox__slider:before {
  background: linear-gradient(to right, $grad-color1, $grad-color2);
  transform: translateX($slider-size);
}

label {
  margin-left: 10px;
  color: #cbcfd4;
  font-size: 14px;
  font-family: "Montserrat", sans-serif;
}
</style>
