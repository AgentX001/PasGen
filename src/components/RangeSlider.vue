<template>
  <div class="range-slider">
    <Label>Length</Label>
    <div class="range-slider__container">
      <input
        class="range-slider__input"
        type="range"
        :min="min"
        :max="max"
        v-bind:value="value"
        v-on:input="onInput"
      >
      <div class="range-slider__cover" :style="coverStyle" @click="onCoverClick"></div>
      <div class="range-slider__output" :style="outputStyle">{{ value }}</div>
    </div>
  </div>
</template>

<script>
import Label from "./Label";

export default {
  name: "RangeSlider",
  components: { Label },
  props: {
    value: Number,
    min: Number,
    max: Number
  },
  data() {
    return {
      thumSize: 20
    };
  },
  computed: {
    coverStyle() {
      // prettier-ignore
      const width = 100 - (100 / (this.max - this.min)) * (this.value - this.min);
      const ml = (this.thumSize * width) / 100;
      return `width: calc(${width}% - ${ml}px);`;
    },
    outputStyle() {
      const left = (100 / (this.max - this.min)) * (this.value - this.min);
      const ml = (this.thumSize * left) / 100;
      return `margin-left: calc(${left}% - ${ml}px);;`;
    }
  },
  methods: {
    onInput(event) {
      this.$emit("input", event.target.value * 1);
    },
    onCoverClick(event) {
      const newValue = Math.round(
        // prettier-ignore
        this.value + (this.max - this.value) * ((event.offsetX + this.thumSize / 2) / event.target.clientWidth)
      );
      this.$emit("input", newValue);
    }
  }
};
</script>

<style lang="scss" scoped>
@import "../vars";

$thumb-size: 20px;
$track-height: 4px;

.range-slider {
  margin-bottom: 32px;
}

.range-slider__container {
  box-sizing: border-box;
  position: relative;
  height: $thumb-size;
}

.range-slider__output {
  margin-top: 8px;
  color: #cbcfd4;
  font-size: 10px;
  font-family: "Montserrat", sans-serif;
  width: $thumb-size;
  text-align: center;
  font-weight: 700;
}

.range-slider__input {
  width: 100%;
  height: $thumb-size;
  border: none;
  background: transparent;

  -webkit-appearance: none;
  outline: none;

  &::-webkit-slider-runnable-track {
    height: 4px;
    background: linear-gradient(to right, $grad-color1, $grad-color2);
    border: none;
  }

  &::-moz-range-track {
    height: 4px;
    background: linear-gradient(to right, $grad-color1, $grad-color2);
  }

  &::-webkit-slider-thumb {
    -webkit-appearance: none;
    cursor: pointer;
    border: none;
    height: $thumb-size;
    width: $thumb-size;
    border-radius: 50%;
    background: #ffffff;
    margin-top: -$thumb-size/2 + $track-height/2;
  }

  &::-moz-range-thumb {
    cursor: pointer;
    border: none;
    height: $thumb-size;
    width: $thumb-size;
    border-radius: 50%;
    background: #ffffff;
  }
}

.range-slider__cover {
  display: block;
  position: absolute;
  top: -2px;
  right: 0;
  margin-top: $thumb-size/2;
  height: $track-height;
  background-color: #747d88;
}
</style>
