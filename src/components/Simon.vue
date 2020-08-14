<template>
  <div class="wrapper">
    <div class="container">
      <h1>Simon Says</h1>
      <div class="simon__content">
        <div class="simon__board">
          <ul class="game-board">
            <li
              class="tile_red"
              sound="1.mp3"
              data-tile="1"
              @click="activeButton"
              :class="{tile_red_active: colorRed}"
            ></li>
            <li
              class="tile_blue"
              sound="2.mp3"
              data-tile="2"
              @click="activeButton"
              :class="{tile_blue_active: colorBlue}"
            ></li>
            <li
              class="tile_yellow"
              sound="3.mp3"
              data-tile="3"
              @click="activeButton"
              :class="{tile_yellow_active: colorYellow}"
            ></li>
            <li
              class="tile_green"
              sound="4.mp3"
              data-tile="4"
              @click="activeButton"
              :class="{tile_green_active: colorGreen}"
            ></li>
          </ul>
        </div>
        <div class="simon__info">
          <div class="game-info">
            <h2>
              Round:
              <span>{{turn}}</span>
            </h2>
            <button @click="start" class="game-info__button">Start</button>
            <p v-if="lose">
              Sorry, but you lose after
              <span>{{turn}}</span> rounds!
            </p>
          </div>
          <div class="game-options">
            <h2>Game Options:</h2>
            <div class="game-options__inputs" v-for="input in inputs" :key="input.id">
              <input
                @change="options"
                type="radio"
                name="mode"
                :value="input.value"
                :checked="input.checked"
              />
              <span>{{input.text}}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    // NEW GAME BUTTON ===================================

    start() {
      if (this.active || this.game) {
        this.lose = false;
        this.play();
      }
    },
    // START GAME =========================================

    play: function () {
      const t = this;

      t.game = false;
      t.order = [];
      t.playerOrder = [];
      t.turn = 1;
      t.flash = 0;
      t.good = true;
      for (let i = 0; i < 100; i++) {
        t.order.push(Math.floor(Math.random() * 4) + 1);
      }
      t.compTurn = true;
      t.intervalId = setInterval(t.gameTurn, t.INTERVAL);
    },
    // GAME TURN ===========================================

    gameTurn: function () {
      const t = this;
      t.active = false;

      if (t.flash == t.turn) {
        clearInterval(t.intervalId);
        t.compTurn = false;
        t.clearColor();
        t.active = true;
      }

      if (t.compTurn) {
        t.clearColor();
        setTimeout(() => {
          if (t.order[t.flash] == 1) t.one();
          if (t.order[t.flash] == 2) t.two();
          if (t.order[t.flash] == 3) t.three();
          if (t.order[t.flash] == 4) t.four();
          t.flash++;
        }, 100);
      }
    },
    // ACTIVE BUTTON ==========================

    activeButton(e) {
      const data = e.target.dataset.tile;
      const t = this;

      if (data == 1) {
        if (t.active) {
          t.one();
          t.playerOrder.push(1);
          t.check();
          if (!t.game) {
            setTimeout(() => {
              this.clearColor();
            }, 200);
          }
        }
      } else if (data == 2) {
        if (t.active) {
          t.two();
          t.playerOrder.push(2);
          t.check();
          if (!t.game) {
            setTimeout(() => {
              this.clearColor();
            }, 200);
          }
        }
      } else if (data == 3) {
        if (t.active) {
          t.three();
          t.playerOrder.push(3);
          t.check();
          if (!t.game) {
            setTimeout(() => {
              this.clearColor();
            }, 200);
          }
        }
      } else if (data == 4) {
        if (t.active) {
          t.four();
          t.playerOrder.push(4);
          t.check();
          if (!t.game) {
            setTimeout(() => {
              this.clearColor();
            }, 200);
          }
        }
      }
    },

    // NUMBER PITE ======================================

    one() {
      if (this.sound) {
        let audio = new Audio(require("../assets/1.mp3"));
        audio.play();
        this.colorRed = true;
      }
      this.sound = true;
    },
    two() {
      if (this.sound) {
        let audio = new Audio(require("../assets/2.mp3"));
        audio.play();
        this.colorBlue = true;
      }
      this.sound = true;
    },
    three() {
      if (this.sound) {
        let audio = new Audio(require("../assets/3.mp3"));
        audio.play();
        this.colorYellow = true;
      }
      this.sound = true;
    },
    four() {
      if (this.sound) {
        let audio = new Audio(require("../assets/4.mp3"));
        audio.play();
        this.colorGreen = true;
      }
      this.sound = true;
    },

    // COLOR RESET ==========================================
    clearColor: function () {
      this.colorRed = false;
      this.colorBlue = false;
      this.colorYellow = false;
      this.colorGreen = false;
    },
    check: function () {
      const t = this;

      if (
        t.playerOrder[t.playerOrder.length - 1] !==
        t.order[t.playerOrder.length - 1]
      ) {
        t.good = false;
      }
      if (t.good == false) {
        t.clearColor();
        t.active = false;
        t.lose = true;
        t.game = true;
      }
      if (t.turn == t.playerOrder.length && t.good && !t.game) {
        t.turn++;
        t.playerOrder = [];
        t.compTurn = true;
        t.flash = 0;
        t.intervalId = setInterval(t.gameTurn, t.INTERVAL);
      }
    },
    // GAME OPTIONS ========================================

    options(e) {
      const target = e.target.value;
      const t = this;
      if (target == "light") {
        t.INTERVAL = 1500;
      } else if (target == "normal") {
        t.INTERVAL = 1000;
      } else if (target == "hight") {
        t.INTERVAL = 400;
      }
    },
  },
  data() {
    return {
      order: [],
      playerOrder: [],
      INTERVAL: 1500,
      game: true,
      lose: false,
      good: false,
      sound: true,
      colorRed: false,
      colorBlue: false,
      colorYellow: false,
      colorGreen: false,
      active: false,
      flash: 0,
      turn: 0,
      inputs: [
        { id: 1, value: "light", text: "Light", checked: "true" },
        { id: 2, value: "normal", text: "Normal" },
        { id: 3, value: "hight", text: "Hight" },
      ],
    };
  },
};
</script>

<style lang="scss">
@import "../style.scss";
</style>
