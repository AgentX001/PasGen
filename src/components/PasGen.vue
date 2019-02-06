<template>
  <div class="pas-gen">
    <span class="pas-gen__output">{{ password }}</span>
    <div class="pas-gen__icons">
      <Icon name="copy" @click="copy"/>
      <Icon name="reload" @click="generate()"/>
    </div>
    <div class="pas-gen__strength" :class="[ strength ]">{{ strength | capitalize }}</div>
  </div>
</template>

<script>
import zxcvbn from "zxcvbn";
import _ from "lodash";

import Icon from "./Icon";

export default {
  name: "PasGen",

  components: { Icon },

  props: {
    digits: Boolean,
    symbols: Boolean,
    length: Number
  },

  data() {
    return {
      password: "",
      strength: ""
    };
  },

  methods: {
    generate() {
      // prettier-ignore
      let lettersSet = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z"];
      // prettier-ignore
      const symbolsSet = [ "=","+","-","^","?","!","%","&","*","$","#","^","@","|"];
      // prettier-ignore
      const digitsSet = ["0", "1", "2", "3", "4", "5", "5", "6", "7", "8", "9"];

      lettersSet = lettersSet.concat(
        lettersSet.map(value => value.toUpperCase())
      );

      let set = lettersSet.concat(
        this.symbols ? symbolsSet : [],
        this.digits ? digitsSet : []
      );

      while (set.length < this.length) {
        set = set.concat(set);
      }

      for (let i = set.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [set[i], set[j]] = [set[j], set[i]];
      }

      this.password = set.slice(0, this.length).join("");
    },

    calcStrength() {
      const { score } = zxcvbn(this.password);
      if (score < 3) {
        this.strength = "average";
      } else if (score === 3) {
        this.strength = "strong";
      } else if (score > 3) {
        this.strength = "secure";
      }
    },

    copy() {
      const tempEl = document.createElement("textarea");
      tempEl.value = this.password;
      tempEl.setAttribute("readonly", "");
      tempEl.style = { position: "absolute", left: "-9999px" };
      document.body.appendChild(tempEl);
      tempEl.select();
      document.execCommand("copy");
      document.body.removeChild(tempEl);
      this.$emit("copy");
    }
  },

  watch: {
    length() {
      this.generate();
    },
    symbols() {
      this.generate();
    },
    digits() {
      this.generate();
    },
    password() {
      this.debouncedCalcStrength();
    }
  },

  filters: {
    capitalize: function(value) {
      if (!value) return "";
      value = value.toString();
      return value.charAt(0).toUpperCase() + value.slice(1);
    }
  },

  created() {
    this.debouncedCalcStrength = _.debounce(this.calcStrength, 100);
    this.generate();
    this.calcStrength();
  }
};
</script>

<style lang="scss"scoped>
.pas-gen {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100px;
  margin-bottom: 32px;
  border-radius: 3px;
  background-color: white;

  &__output {
    max-width: 205px;
    word-wrap: break-word;
    text-align: center;
    font-size: 14px;
    line-height: 22px;
    font-family: "Montserrat", sans-serif;
  }

  &__icons {
    position: absolute;
    right: 0;
  }

  &__strength {
    position: absolute;
    display: block;
    width: 100%;
    left: 0;
    bottom: 0;
    margin-bottom: -18px;
    font-size: 10px;
    font-family: "Montserrat", sans-serif;

    &.secure {
      color: #6fcf97;
    }
    &.strong {
      color: #a154f2;
    }
    &.average {
      color: #cf6f8a;
    }

    &::after {
      content: " ";
      position: absolute;
      display: block;
      top: 0;
      left: 0;
      margin-top: -10px;
      width: 100%;
      height: 4px;
      border-bottom-right-radius: 3px;
      border-bottom-left-radius: 3px;
    }

    &.secure::after {
      background-color: #6fcf97;
    }
    &.strong::after {
      background-color: #a154f2;
    }
    &.average::after {
      background-color: #cf6f8a;
    }
  }
}
</style>
