<template>
  <button class="btn" @click="restart">
    {{ text }}
    <div class="hov"></div>
  </button>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  props: ["text"],
  methods: {
    restart() {
      this.$emit("restart");
    },
  },
});
</script>

<style lang="scss">
.btn {
  width: 150px;
  height: 50px;
  background-color: #595d6e;
  border-style: hidden;
  border-radius: 10px;
  font-family: "Poppins", sans-serif;
  color: white;
  font-size: 20px;
  transition: 200ms;
  cursor: pointer;
  position: relative;
  z-index: 10;
  overflow: hidden;
}
.btn:hover {
  letter-spacing: 2px;
  box-shadow: 0px 0px 15px 0px #43a74d;
  transition: 200ms;
}
.btn:active {
  transform: scale(0.95);
  transition: 100ms;
}
.hov {
  content: "";
  width: 100%;
  height: 0%;
  background-color: #43a74d;
  position: absolute;
  bottom: 0;
  left: 0;

  z-index: -1;
  border-radius: 10px;
  transition: 200ms;
}

.btn:hover > .hov {
  transition: 200ms;
  width: 100%;
  height: 100%;
  background-color: #43a74d;
  position: absolute;
  bottom: 0;
  left: 0;
  z-index: -1;
  border-radius: 10px;
}
</style>
