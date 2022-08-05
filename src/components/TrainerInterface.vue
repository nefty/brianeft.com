<template>
  <div class="interface">
    <div id="button-grid">
      <h2 class="title" v-if="isSessionActive">Select the Correct Square</h2>
      <h2 class="title" v-else>Session Ended</h2>
      <div class="success-image" v-if="isSuccessfulTrial">
        <img :src="getRandomImage()" />
        <audio autoplay>
          <source src="@/assets/winner.mp3" />
        </audio>
      </div>
      <button
        :class="{ flashGreen: isColorCorrect.green }"
        class="color-button"
        id="green"
        @click="select"
      ></button>
      <button
        :class="{ flashYellow: isColorCorrect.yellow }"
        class="color-button"
        id="yellow"
        @click="select"
      ></button>
      <button
        :class="{ flashRed: isColorCorrect.red }"
        class="color-button"
        id="red"
        @click="select"
      ></button>
      <button
        :class="{ flashBlue: isColorCorrect.blue }"
        class="color-button"
        id="blue"
        @click="select"
      ></button>
      <button
        class="pass-reset"
        id="pass"
        v-if="isSessionActive"
        @click="select"
      >
        Pass
      </button>
      <button class="pass-reset" id="reset" v-else @click="reset">
        New Session
      </button>
    </div>
    <p>Color Selections: <strong>{{ colorSelections }}/24</strong></p>
    <p>Current Score: <strong>{{ score }}</strong></p>
  </div>
</template>

<script>
import win1 from "@/assets/stephen-leonardi.jpg";
import win2 from "@/assets/christian-schrader.jpg";
import win3 from "@/assets/darkroomlabs.jpg";
import win4 from "@/assets/marek-piwnicki.jpg";
import win5 from "@/assets/wenhao-ryan.jpg";
import win6 from "@/assets/sajad-nori.jpg";

export default {
  data() {
    return {
      session: [],
      score: 0,
      isSuccessfulTrial: false,
      isSessionActive: true,
      isColorCorrect: {
        green: false,
        yellow: false,
        red: false,
        blue: false,
      },
      images: [],
    };
  },
  created() {
    this.images.push(win1);
    this.images.push(win2);
    this.images.push(win3);
    this.images.push(win4);
    this.images.push(win5);
    this.images.push(win6);
  },
  methods: {
    select(event) {
      if (!this.isSessionActive) {
        return;
      }

      const sessionLength = 24;
      const successTimeout = 800;
      const flashInterval = 201;

      let trial = {};
      trial.computerSelection = this.chooseColor();
      trial.userSelection = event.target.id;

      if (trial.userSelection === trial.computerSelection) {
        this.score++;
        this.isSuccessfulTrial = true;
        setTimeout(() => {
          this.isSuccessfulTrial = false;
        }, successTimeout);
      } else {
        this.isColorCorrect[trial.computerSelection] = true;
        setTimeout(() => {
          this.isColorCorrect[trial.computerSelection] = false;
        }, flashInterval);
      }

      this.session.push(trial);

      if (this.colorSelections === sessionLength) {
        this.isSessionActive = false;
      }
    },
    chooseColor() {
      let randomColor = Math.floor(Math.random() * 4);
      switch (randomColor) {
        case 0:
          return "green";
        case 1:
          return "yellow";
        case 2:
          return "red";
        case 3:
          return "blue";
      }
    },
    getRandomImage() {
      let imageIndex = Math.floor(Math.random() * this.images.length);
      console.log(this.images[imageIndex]);
      return this.images[imageIndex];
    },
    reset() {
      this.isSessionActive = true;
      this.session = [];
      this.score = 0;
    },
  },

  computed: {
    colorSelections() {
      let colorSelections = this.session.filter((trial) => {
        return trial.userSelection !== "pass";
      });
      return colorSelections.length;
    },
  },
};
</script>

<style scoped>
.title {
  grid-column: 1 / 3;
  padding-top: 0.5rem;
}

#button-grid {
  margin: 12px auto 0 auto;
  display: grid;
  grid-template-columns: 50% 50%;
  max-width: 480px;
  position: relative;
}

.interface {
  text-align: center;
  background-color: var(--color-background-mute);
  border-radius: 12px;
  padding-bottom: 0.5rem;
}

.interface p {
  padding: 0;
}

.color-button {
  border: none;
  border-radius: 12px;
  margin: 12px;
  display: inline-block;
  font-size: 16px;
  aspect-ratio: 1;
}

button:active {
  transform: translateY(4px);
}

.pass-reset {
  text-align: center;
  border: none;
  margin: 12px;
  padding: 6px;
  border-radius: 12px;
  color: var(--color-background-mute);
  background-color: var(--color-text);
  font-size: 2em;
  grid-column: 1 / 3;
}

#pass:active {
  background-color: var(--vt-c-indigo-light);
}

@keyframes blinking-green {
  0% {
    background-color: var(--vt-c-green-light);
  }
  50% {
    background-color: var(--vt-c-green);
  }
  100% {
    background-color: var(--vt-c-green-light);
  }
}

@keyframes blinking-yellow {
  0% {
    background-color: var(--vt-c-yellow-light);
  }
  50% {
    background-color: var(--vt-c-yellow);
  }
  100% {
    background-color: var(--vt-c-yellow-light);
  }
}

@keyframes blinking-red {
  0% {
    background-color: var(--vt-c-red-light);
  }
  50% {
    background-color: var(--vt-c-red);
  }
  100% {
    background-color: var(--vt-c-red-light);
  }
}

@keyframes blinking-blue {
  0% {
    background-color: var(--vt-c-blue-light);
  }
  50% {
    background-color: var(--vt-c-blue);
  }
  100% {
    background-color: var(--vt-c-blue-light);
  }
}

#green {
  background-color: var(--vt-c-green);
}

#green:active {
  background-color: var(--vt-c-green-light);
}

.flashGreen {
  animation: blinking-green 200ms;
  animation-timing-function: linear;
  animation-fill-mode: backwards;
}

#yellow {
  background-color: var(--vt-c-yellow);
}

#yellow:active {
  background-color: var(--vt-c-yellow-light);
}

.flashYellow {
  animation: blinking-yellow 200ms;
  animation-timing-function: linear;
  animation-fill-mode: backwards;
}

#red {
  background-color: var(--vt-c-red);
}

#red:active {
  background-color: var(--vt-c-red-light);
}

.flashRed {
  animation: blinking-red 200ms;
  animation-timing-function: linear;
  animation-fill-mode: backwards;
}

#blue {
  background-color: var(--vt-c-blue);
}

#blue:active {
  background-color: var(--vt-c-blue-light);
}

.flashBlue {
  animation: blinking-blue 200ms;
  animation-timing-function: linear;
  animation-fill-mode: backwards;
}

.success-image {
  position: absolute;
  top: 0px;
  left: 0px;
  margin: 12px;
  z-index: 1;
  grid-row: 2 / 4;
}

.success-image img {
  width: 100%;
  border-radius: 12px;
}

@media (max-height: 800px) {
  #button-grid {
    max-width: 300px;
  }
}
</style>
