<template>
  <div id="app">
    <main>
      <h1 v-if="!showCopiedMsg">Password Generator</h1>
      <h1 v-else>Copied!</h1>
      <PasGen :digits="digits" :symbols="symbols" :length="length" v-on:copy="onCopy"/>
      <RangeSlider v-model="length" :min="8" :max="64"/>
      <Label>Settings</Label>
      <div class="checkboxes__container">
        <div class="checkboxes">
          <Checkbox v-model="digits" label="Use digits"/>
          <Checkbox v-model="symbols" label="Use symbols"/>
        </div>
      </div>
    </main>
    <a href="https://github.com/AgentX001/PasGen" class="github-link">GitHub</a>
  </div>
</template>

<script>
import PasGen from "./components/PasGen";
import RangeSlider from "./components/RangeSlider";
import Label from "./components/Label";
import Checkbox from "./components/Checkbox";

export default {
  name: "App",
  components: { PasGen, RangeSlider, Label, Checkbox },
  data() {
    return {
      length: 12,
      digits: true,
      symbols: false,
      showCopiedMsg: false,
    };
  },
  methods: {
    onCopy() {
      this.showCopiedMsg = true;
      setTimeout(() => this.showCopiedMsg = false, 1000);
    }
  }
};
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css?family=Montserrat|Zilla+Slab");

@import "vars";

#app {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-image: linear-gradient(
      0deg,
      rgba($bg-color, 0.3),
      rgba($bg-color, 0.3)
    ),
    url("./assets/background.jpg");
  background-size: cover;
  background-position: center center;
}

main {
  width: 95%;
  box-sizing: border-box;
  max-width: 425px;
  padding: 32px;
  border-radius: 6px;
  background-color: #3c444d;
}

h1 {
  margin: 0 0 40px 0;
  text-align: center;
  color: white;
  font-family: "Zilla Slab", serif;
  font-size: 24px;
  font-weight: normal;
}

.checkboxes__container {
  display: flex;
  justify-content: center;
}

.checkboxes {
  width: 300px;
  max-width: 100%;
  display: flex;
  justify-content: space-between;
}

.github-link {
  position: absolute;
  display: block;
  bottom: 12px;
  color: #cbcfd4;
  font-size: 12px;
  font-family: "Montserrat", sans-serif;
  text-decoration: none;
  transition: 0.4s;

  &:hover {
    opacity: 0.8;
  }
}

@media screen and (max-width: 375px) {
  .checkboxes {
    flex-direction: column;
    height: 48px;
  }
}

@media screen and (max-height: 540px) {
  .github-link {
    display: none;
  }
}
</style>
