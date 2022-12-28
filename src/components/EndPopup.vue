<template>
  <TransitionGroup
    name="popup-out"
    @enter="isAnimationed = true"
    @before-leave="isAnimationed = false"
  >
    <div v-if="open" class="popup">
      <Transition name="popup-in">
        <div v-if="isAnimationed" class="popup_inner">
          <div class="win">
            <div v-if="win" class="win_string">YOU WIN</div>
            <div v-if="lose" class="win_string">GAME OVER</div>
            <span v-if="win" class="scorewin"
              >SCORE <br />
              {{ score }}</span
            >
            <Button @restart="restart" text="PLAY AGAIN"></Button>
          </div>
        </div>
      </Transition>
    </div>
  </TransitionGroup>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Button from "@/components/UI/Button.vue";
export default defineComponent({
  components: { Button },
  props: ["score", "win", "lose", "open"],
  data() {
    return {
      isAnimationed: false,
    };
  },

  methods: {
    restart() {
      this.$emit("restart");
    },
  },
});
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@1,400;1,500;1,600&family=Poppins:wght@500&family=Sevillana&display=swap");

.popup {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 100;
  .popup_inner {
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: "Poppins", sans-serif;
    font-size: 25px;
    width: 30%;
    height: 25%;
    background-color: #474b59;
    border-radius: 10px;
  }
  .scorewin {
    text-align: center;
  }
  .win {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .win_string {
    font-size: 30px;
    margin-bottom: 10px;
    text-align: center;
  }
}

.popup-out-enter-active,
.popup-out-leave-active {
  transition: all 0.5s ease;
  transition-duration: 1s;
}

.popup-out-enter-from,
.popup-out-leave-to {
  opacity: 0;
}
.popup-in-enter-active,
.popup-in-leave-active {
  transform: scale(1);
  transition-duration: 500ms;
}

.popup-in-enter-from,
.popup-in-leave-to {
  transform: scale(0);
  transition-duration: 500ms;
}
</style>
