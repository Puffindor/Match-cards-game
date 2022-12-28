<template>
  <div v-if="!isStarted" class="popup_out">
    <div class="popup_inner">
      <div class="con">
        <div class="mode">
          <div @click="switchMode" class="options">
            <div class="curcle"></div>
          </div>
          <span>
            *Use
            <span>{{ modeString }}</span>
          </span>
          <span> as card background</span>
        </div>
        <div class="selector">
          <div @click="down">
            <Arrow :rotation="`180deg`" />
          </div>
          <div class="diff">
            <Transition name="easy">
              <div v-if="difficulty === 1">EASY</div>
            </Transition>
            <Transition name="med">
              <div v-if="difficulty === 2">MEDIUM</div>
            </Transition>
            <Transition name="hard">
              <div v-if="difficulty === 3">HARD</div>
            </Transition>
          </div>
          <div @click="up">
            <Arrow :rotation="`0deg`" />
          </div>
        </div>
      </div>
      <Button @restart="start" text="START"></Button>
    </div>
  </div>
</template>

<script lang="ts" scoped>
import Arrow from "@/assets/arrow.vue";
import { defineComponent } from "vue";
import Button from "./UI/Button.vue";
export default defineComponent({
  data() {
    return {
      difficulty: 1,
      mode: 2,
    };
  },
  props: ["isStarted"],
  components: { Arrow, Button },
  methods: {
    switchMode() {
      if (this.mode < 3) {
        console.log(213);
        this.mode += 1;
      } else {
        this.mode = 1;
      }
    },
    up() {
      if (this.difficulty < 3) {
        this.difficulty += 1;
      }
    },
    down() {
      if (this.difficulty > 1) {
        this.difficulty -= 1;
      }
    },
    start() {
      this.$emit("start", this.difficulty, this.mode);
    },
  },
  computed: {
    modeString() {
      let num: number = this.mode;
      switch (num) {
        case 1:
          return "Numbers";
        case 2:
          return "Icons";
        case 3:
          return "Random pictures";
      }
    },
    curclePos() {
      let num: number = this.mode;
      switch (num) {
        case 1:
          return "5px";
        case 2:
          return "23px";
        case 3:
          return "40px";
      }
    },
  },
});
</script>

<style lang="scss" scoped>
@mixin to {
}
@mixin from {
  transform: translateX(-400px);
  transition: 1100ms;
}
@mixin options {
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  transition: 200ms ease;
  width: 60px;
  height: 25px;
  background-color: #595d6e;
  border-radius: 20px;
  margin-right: 20px;
}
.curcle {
  transition-duration: 200ms;
  height: 15px;
  width: 15px;
  border-radius: 100%;
  background-color: #43a74d;
  transform: translateX(v-bind(curclePos));
}
span {
  font-size: 12px;
}
.options {
  @include options;
}
.options:hover > .curcle {
  background-color: #4cbc57;
  box-shadow: 0px 0px 10px 5px #43a74d;
}
.mode {
  padding: 9px;
  display: flex;
  width: 100%;
  height: 43px;
  background-color: #717586;
  border-radius: 20px;
  align-items: center;
  margin-bottom: 15px;
  span {
    font-size: 16px;
    span {
      font-size: 16px;
      color: #43a74d;
    }
  }
}
.diff {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: relative;
}
.selector {
  display: grid;
  grid-template-columns: 48px 1fr 48px;
  width: 100%;
  height: 30%;
  background-color: #717586;
  border-radius: 20px;
  overflow: hidden;
  position: relative;
}
.popup_out {
  user-select: none;
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 100;
}
.popup_inner {
  color: white;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-direction: column;
  font-family: "Poppins", sans-serif;
  font-size: 25px;
  width: 25%;
  height: 20%;
  background-color: #474b59;
  border-radius: 10px;
  padding: 10px;
  .con {
    height: 80%;
    width: 100%;
  }
}

.easy-leave-active {
  transition: 200ms;
  position: absolute;
}
.easy-enter-active {
  transition: 200ms;
}
.easy-leave-from {
  transform: translateX(0px);
}
.easy-leave-to {
  transform: translateX(-400px);
  align-self: center;
}

.easy-enter-from {
  transform: translateX(400px);
  align-self: center;
}
.easy-enter-to {
  transform: translateX(0px);
}

.med-leave-active {
  transition: 200ms;
  position: absolute;
}
.med-enter-active {
  transition: 200ms;
}
.med-leave-from {
  transform: translateX(0px);
}
.med-leave-to {
  transform: translateX(-400px);
}

.med-enter-from {
  transform: translateX(400px);
}
.med-enter-to {
  transform: translateX(0px);
}

.hard-leave-active {
  transition: 200ms;
  position: absolute;
}
.hard-enter-active {
  transition: 200ms;
}
.hard-leave-from {
  transform: translateX(0px);
}
.hard-leave-to {
  transform: translateX(-400px);
}

.hard-enter-from {
  transform: translateX(400px);
}
.hard-enter-to {
  transform: translateX(0px);
}
</style>
