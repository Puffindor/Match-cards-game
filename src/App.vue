<template>
  <div class="container">
    <end-popup
      @restart="restart"
      :score="score"
      :open="over"
      :lose="lose"
      :win="win"
    />
    <StartPopup @start="start" :isStarted="isStarted" />
    <div class="score">
      SCORE <span class="multiplier">x{{ scoreMultiplier }}</span>
      <br />
      <div>
        {{ score }}
      </div>
    </div>
    <div v-if="isStarted" class="cards">
      <div v-for="card in cards" :key="card.id">
        <Card
          :value="card.value"
          :id="card.id"
          :isOpen="card.isOpen"
          :isSet="card.isSet"
          :useIcons="useIcons"
          :test="test"
          :usePictures="usePictures"
          :loaded="loaded"
          @Open="openCard"
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Card from "@/components/Card.vue";
import EndPopup from "./components/EndPopup.vue";
import StartPopup from "./components/Startpopup.vue";
type Card = {
  id: number;
  isSet: boolean;
  isOpen: boolean;
  value: number;
};
export default defineComponent({
  data() {
    return {
      test: [] as Array<string | null>,
      cards: [] as Card[],
      loaded: false,
      usePictures: false,
      useIcons: false,
      numColums: 0,
      scoreMultiplier: 0,
      score: 5,
      correct: false,
      values: [] as number[],
      openedCards: [] as number[],
      conunter: 0,
      isStarted: false,
      over: false,
      lose: false,
      win: false,
    };
  },
  components: { Card, EndPopup, StartPopup },
  methods: {
    async getPicture(num: number) {
      try {
        const promises = [];
        let x = 1;
        this.test = [];
        while (x < num / 2 + 1) {
          await promises.push(fetch("https://picsum.photos/150/150"));
          x++;
        }
        await Promise.all(promises).then((response) => {
          response.forEach((el) => this.test.push(el.url));
        });
        this.test.unshift(null);
        this.loaded = true;
      } catch (err) {
        alert(
          "Failed to load images, it looks like you have problems with your internet connection"
        );
        this.restart();
      }
    },

    generate(cells: number) {
      if (this.usePictures) {
        this.getPicture(cells);
      } else {
        this.loaded = true;
      }
      let i = 1;
      this.cards.shift();
      this.values = [];
      while (i < cells / 2 + 1) {
        this.values.push(i);
        i++;
      }

      i = 0;
      this.values = [...this.values, ...this.values];
      this.values = this.shuffle(this.values);
      if (this.usePictures) {
      }
      while (i < cells) {
        this.cards.push({
          id: Math.random(),
          isSet: false,
          isOpen: false,
          value: this.values[i],
        });
        i++;
      }
      this.isStarted = true;
    },

    start(dif: number, mode: number) {
      switch (mode) {
        case 1:
          break;
        case 2:
          this.useIcons = true;
          break;
        case 3:
          this.usePictures = true;
          break;
      }
      switch (dif) {
        case 1:
          this.generate(16);
          this.numColums = 4;
          this.scoreMultiplier = 0.8;
          break;
        case 2:
          this.generate(20);
          this.numColums = 5;
          this.scoreMultiplier = 1;
          break;
        case 3:
          this.generate(24);
          this.numColums = 6;
          this.scoreMultiplier = 2;
          break;
      }
    },
    end() {
      if (this.score <= 0) {
        this.lose = true;
        this.over = true;
      }

      if (
        this.cards.filter((el) => el.isSet === true).length ===
        this.cards.length
      ) {
        this.win = true;
        this.over = true;
      }
    },
    restart() {
      this.usePictures = false;
      this.useIcons = false;
      this.win = false;
      this.lose = false;
      this.over = false;
      this.isStarted = false;
      this.score = 5;
      this.cards.forEach((el) => {
        el.isOpen = false;
        el.isSet = false;
      });
      this.cards = [];
      this.values = [];
    },
    async openCard(id: number) {
      if (this.conunter < 2) {
        this.cards.forEach((el) => {
          if (el.id === id) {
            el.isOpen = true;

            this.openedCards.push(this.cards.indexOf(el));
          }
        });
        if (this.openedCards[0] === this.openedCards[1]) {
          this.openedCards.pop();
          return;
        }
        this.conunter += 1;
        if (this.conunter === 2) {
          if (
            this.cards[this.openedCards[0]].value ===
            this.cards[this.openedCards[1]].value
          ) {
            this.cards[this.openedCards[0]].isSet = true;
            this.cards[this.openedCards[1]].isSet = true;
            this.score += 10 * this.scoreMultiplier;
            this.correct = true;
          }
          await this.sleep();

          this.cards.forEach((el) => (el.isOpen = false));
          if (!this.correct) {
            this.score -= 1;
          }
          this.correct = false;
          this.conunter = 0;
          this.openedCards = [];
          this.end();
        }
      }
    },

    shuffle(array: number[]) {
      let currentIndex = array.length,
        temporaryValue,
        randomIndex;
      while (0 !== currentIndex) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }

      return array;
    },
    sleep() {
      return new Promise((resolve) => setTimeout(resolve, 700));
    },
  },
});
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@1,400;1,500;1,600&family=Poppins:wght@500&family=Sevillana&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.multiplier {
  color: #43a74d;
  font-size: 20px;
}
.score {
  color: white;
  font-family: "Poppins", sans-serif;
  text-align: center;
  font-size: 30px;
  margin-bottom: 20px;
}
.cards {
  display: grid;
  grid-template-columns: repeat(v-bind(numColums), 1fr);
  grid-gap: 10px;
}
.container {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  background-color: #4c636d;
}
</style>
