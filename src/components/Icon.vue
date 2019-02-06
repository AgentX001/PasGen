<template>
  <i @click="onClick">
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24"
      height="24"
      viewBox="0 0 24 24"
      v-if="name=='copy'"
    >
      <path fill="none" d="M0 0h24v24H0z"></path>
      <path
        d="M16 1H4c-1.1 0-2 .9-2 2v14h2V3h12V1zm-1 4l6 6v10c0 1.1-.9 2-2 2H7.99C6.89 23 6 22.1 6 21l.01-14c0-1.1.89-2 1.99-2h7zm-1 7h5.5L14 6.5V12z"
      ></path>
    </svg>
    
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24"
      height="24"
      viewBox="0 0 24 24"
      v-if="name=='reload'"
    >
      <path
        d="M17.65 6.35C16.2 4.9 14.21 4 12 4c-4.42 0-7.99 3.58-7.99 8s3.57 8 7.99 8c3.73 0 6.84-2.55 7.73-6h-2.08c-.82 2.33-3.04 4-5.65 4-3.31 0-6-2.69-6-6s2.69-6 6-6c1.66 0 3.14.69 4.22 1.78L13 11h7V4l-2.35 2.35z"
      ></path>
      <path d="M0 0h24v24H0z" fill="none"></path>
    </svg>
  </i>
</template>

<script>
export default {
  name: "Icon",
  props: {
    name: String
  },
  methods: {
    onClick(e) {
      this.$emit("click");
    }
  }
};
</script>

<style lang="scss" scoped>
i {
  display: block;
  width: 24px;
  height: 24px;
  margin: 12px 12px;
  fill: #f27a54;
  cursor: pointer;
  transition: all 0.3s;

  &:hover {
    transform: scale(1.05);
    opacity: 0.9;
  }
}
</style>
