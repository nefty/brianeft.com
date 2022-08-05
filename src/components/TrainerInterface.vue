<template>
  <div class="interface">
    <div id="button-grid">
      <h2 class="title" v-if="isSessionActive">Select the Correct Square</h2>
      <h2 class="title" v-else>Session Ended</h2>
      <div class="success-image" v-if="isSuccessfulTrial">
        <img src="@/assets/stephen-leonardi.jpg" />
        <audio autoplay>
          <source src="@/assets/winner.mp3" />
        </audio>
      </div>
      <button
        :class="{ activeGreen: isGreenCorrect }"
        class="color-button"
        id="green"
        @click="select"
      ></button>
      <button
        :class="{ activeYellow: isYellowCorrect }"
        class="color-button"
        id="yellow"
        @click="select"
      ></button>
      <button
        :class="{ activeRed: isRedCorrect }"
        class="color-button"
        id="red"
        @click="select"
      ></button>
      <button
        :class="{ activeBlue: isBlueCorrect }"
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
    <p>Color Selections: {{ colorSelections }}/24</p>
    <p>Current Score: {{ score }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      session: [],
      score: 0,
      isSuccessfulTrial: false,
      isSessionActive: true,
      isGreenCorrect: false,
      isYellowCorrect: false,
      isRedCorrect: false,
      isBlueCorrect: false,
    };
  },

  methods: {
    select(event) {
      if (!this.isSessionActive) {
        return;
      }

      const sessionLength = 24;
      const successTimeout = 800;

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
        this.flashCorrectColor(trial.computerSelection);
      }

      this.session.push(trial);

      if (this.colorSelections === sessionLength) {
        console.log(this.colorSelections);
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
    reset() {
      this.isSessionActive = true;
      this.session = [];
      this.score = 0;
    },
    flashCorrectColor(correctColor) {
      const flashInterval = 100;
      const flashTimeout = 500;

      switch (correctColor) {
        case "green": {
          let flashTimer = setInterval(() => {
            this.isGreenCorrect = !this.isGreenCorrect;
          }, flashInterval);
          setTimeout(() => {
            clearInterval(flashTimer);
          }, flashTimeout);
          break;
        }
        case "yellow": {
          let flashTimer = setInterval(() => {
            this.isYellowCorrect = !this.isYellowCorrect;
          }, flashInterval);
          setTimeout(() => {
            clearInterval(flashTimer);
          }, flashTimeout);
          break;
        }
        case "red": {
          let flashTimer = setInterval(() => {
            this.isRedCorrect = !this.isRedCorrect;
          }, flashInterval);
          setTimeout(() => {
            clearInterval(flashTimer);
          }, flashTimeout);
          break;
        }
        case "blue": {
          let flashTimer = setInterval(() => {
            this.isBlueCorrect = !this.isBlueCorrect;
          }, flashInterval);
          setTimeout(() => {
            clearInterval(flashTimer);
          }, flashTimeout);
          break;
        }
      }
      this.isGreenCorrect = false;
      this.isYellowCorrect = false;
      this.isRedCorrect = false;
      this.isBlueCorrect = false;
    },
  },

  computed: {
    colorSelections() {
      let colorSelections = this.session.filter((trial) => {
        return trial.userSelection !== "pass";
      });
      return colorSelections.length;
    },
    getRandomImage() {
      const path = "@/assets/";
      const images = ["stephen-leonardi.jpg"];

      let imageIndex = Math.floor(Math.random() * images.length);
      console.log(path + images[imageIndex]);
      return path + images[imageIndex];
    },
  },
};
</script>

<style scoped>
.title {
  grid-column: 1 / 3;
}

#button-grid {
  margin: 12px auto;
  display: grid;
  grid-template-columns: 50% 50%;
  max-width: 480px;
  position: relative;
}

.interface {
  text-align: center;
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
  color: var(--vt-c-white-mute);
  background-color: var(--vt-c-indigo);
  font-size: 2em;
  grid-column: 1 / 3;
}

#pass:active {
  background-color: var(--vt-c-indigo-light);
}

#green {
  background-color: var(--vt-c-green);
}

#green:active {
  background-color: var(--vt-c-green-light);
}

.activeGreen {
  background-color: var(--vt-c-green-light) !important;
}

#yellow {
  background-color: var(--vt-c-yellow);
}

#yellow:active {
  background-color: var(--vt-c-yellow-light);
}

.activeYellow {
  background-color: var(--vt-c-yellow-light) !important;
}

#red {
  background-color: var(--vt-c-red);
}

#red:active {
  background-color: var(--vt-c-red-light);
}

.activeRed {
  background-color: var(--vt-c-red-light) !important;
}

#blue {
  background-color: var(--vt-c-blue);
}

#blue:active {
  background-color: var(--vt-c-blue-light);
}

.activeBlue {
  background-color: var(--vt-c-blue-light) !important;
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
