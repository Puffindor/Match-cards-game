<template>
  <div class="con">
    <div
      @click="open"
      :class="{ card_open: isOpen || isSet, card: !isOpen && !isSet }"
    >
      <div class="front">
        <Transition name="load">
          <div v-if="!test[value] && usePictures">
            <loading />
          </div>
        </Transition>
        <Transition name="load2">
          <div v-if="test[value] || !usePictures">
            <question />
          </div>
        </Transition>
      </div>
      <div class="back">
        <span v-if="!useIcons && !usePictures">{{ value }}</span>
        <img v-if="usePictures" :src="test[value]" />
        <component :is="name" v-if="useIcons" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import loading from "@/assets/loading.vue";
import question from "@/assets/question.vue";
import a1 from "@/assets/card icons/a1.vue";
import a2 from "@/assets/card icons/a2.vue";
import a3 from "@/assets/card icons/a3.vue";
import a4 from "@/assets/card icons/a4.vue";
import a5 from "@/assets/card icons/a5.vue";
import a6 from "@/assets/card icons/a6.vue";
import a7 from "@/assets/card icons/a7.vue";
import a8 from "@/assets/card icons/a8.vue";
import a9 from "@/assets/card icons/a9.vue";
import a10 from "@/assets/card icons/a10.vue";
import a11 from "@/assets/card icons/a11.vue";
import a12 from "@/assets/card icons/a12.vue";
import { defineComponent } from "vue";

export default defineComponent({
  components: {
    question,
    a1,
    a2,
    a3,
    a4,
    a5,
    a6,
    a7,
    a8,
    a9,
    a10,
    a11,
    a12,
    loading,
  },
  props: [
    "value",
    "id",
    "isOpen",
    "isSet",
    "useIcons",
    "test",
    "usePictures",
    "loaded",
  ],
  data() {
    return {
      name: `a${this.value}`,
    };
  },
  methods: {
    open() {
      if (this.loaded) {
        this.$emit("Open", this.id);
      }
    },
  },
  computed: {
    iconName() {
      if (!this.test[this.value] && this.usePictures) {
        return "loading";
      }
      if (this.test[this.value] || !this.usePictures) {
        return "question";
      }
    },
  },
});
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@1,400;1,500;1,600&family=Poppins:wght@500&family=Sevillana&display=swap");
img {
  border-radius: 10px;
}
.con {
  color: white;
  font-family: "Poppins", sans-serif;
  width: 150px;
  height: 150px;
  transition: 200ms;
  transform-style: preserve-3d;
}

.card_open {
  width: 150px;
  height: 150px;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  position: relative;
  transition: transform 1500ms;
  transform-style: preserve-3d;
  transform: rotateY(180deg);
  transition: 1000ms;
}
.front {
  width: 100%;
  height: 100%;
  border-radius: 10px;
  background-color: #474b59;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  backface-visibility: hidden;
}
.back {
  font-size: 30px;
  width: 100%;
  height: 100%;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #43a74d;
  transform: rotateY(180deg);
  position: absolute;
  backface-visibility: hidden;
}
.card {
  width: 150px;
  height: 150px;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  position: relative;
  transition: box-shadow 300ms, transform 1000ms;
  transform-style: preserve-3d;
}
.card:hover {
  width: 150px;
  height: 150px;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  position: relative;
  transition: box-shadow 300ms, transform 1000ms;
  transform-style: preserve-3d;
  transform: scale(1.02);
  box-shadow: 0px 0px 15px 2px rgba(0, 0, 0, 0.5);
}

.load-leave-active {
  transition: all 200ms;
}

.load-leave-from {
  opacity: 1;
}

.load-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

.load2-enter-active {
  transition: transform 200ms ease-out;
  transition-delay: 200ms;
}
.load2-enter-to,
.load2-leave-from {
  transform: scale(1);
}
.load2-enter-from,
.load2-leave-to {
  transform: scale(0);
}
</style>
