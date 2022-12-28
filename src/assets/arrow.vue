<template>
  <div>
    <svg
      xmlns="http://www.w3.org/2000/svg"
      height="48"
      width="48"
      fill="#ffffffff"
    >
      <path d="m18.75 36-2.15-2.15 9.9-9.9-9.9-9.9 2.15-2.15L30.8 23.95Z" />
    </svg>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  props: ["rotation"],
});
</script>
<style scoped>
div {
  position: absolute;
  overflow: hidden;
  z-index: 100;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
  transition: 200ms;
  cursor: pointer;
}
div:hover {
  position: absolute;
  overflow: hidden;
  z-index: 100;
  transition: 200ms;
  background-color: #878ca2;
}
svg {
  transform: rotate(v-bind (rotation));
}
</style>
