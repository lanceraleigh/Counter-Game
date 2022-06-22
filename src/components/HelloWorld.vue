<template>
  <h2>Current Multipilier: {{ multiplier }}x</h2>
  <h2>Current Passive Income: ${{ passiveIncomeRate }} /sec</h2>
  <p>${{ totalDisplay }}</p>
  <button
    @click="primaryButton()"
    @mousedown="animationUp()"
    @mouseup="animationDown()"
    id="mainButton"
  >
    <p>${{ multiplier }}</p>
  </button>
  <button @click="upgradeSpeed">
    2x passive speed for ${{ intervalCost }}
  </button>
  <button @click="upgradeMultiplier">
    2x passive income for ${{ multiplierCost }}
  </button>
  <button @click="saveProgress">Save</button>
  <button @click="restart">Reset All</button>

  <!-- <button @click="firstMultiplier" id="first">10x COST $100</button>
  <button @click="secondMultiplier" id="second">100x COST $1000</button>
  <button @click="thirdMultiplier" id="third">500x COST $50,000</button>
  <button @click="fourthMultiplier" id="fourth">1000x COST $250,000</button>
  <button @click="fifthMultiplier" id="fifth">2500x COST $1,000,000</button> -->
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      totalDisplay: 0,
      multiplier: 1,
      interval: 5000,
      multiplierCost: 10,
      intervalCost: 10,
    };
  },
  computed: {
    passiveIncomeRate() {
      return this.multiplier / (this.interval / 1000);
    },
  },

  // Turn this into a single function with local storage saving capabilities
  methods: {
    abbreviateNumber(value) {
      let newValue = value;
      if (value >= 1000) {
        let suffixes = ["", "k", "m", "b", "t"];
        let suffixNum = Math.floor(("" + value).length / 3);
        let shortValue = "";
        for (let precision = 2; precision >= 1; ) {
          shortValue = parseFloat(
            (suffixNum !== 0
              ? value / Math.pow(1000, suffixNum)
              : value
            ).toPrecision(precision)
          );
          let dotLessShortValue = (shortValue + "").replace(
            /[^a-zA-Z 0-9]+/g,
            ""
          );
          if (dotLessShortValue.length <= 2) {
            break;
          }
        }
        if (shortValue % 1 !== 0) shortValue = shortValue.toFixed(1);
        newValue = shortValue + suffixes[suffixNum];
      }
      return newValue;
    },
    newCount() {
      setInterval(this.primaryButton, this.interval);
    },
    primaryButton() {
      // let total = this.totalDisplay + this.multiplier;
      // let summarized = this.abbreviateNumber(total);
      // this.totalDisplay = summarized;

      this.totalDisplay = this.totalDisplay + this.multiplier;
    },
    animationUp() {
      document.querySelector("#mainButton").style.transform = "scale(1.05)";
    },
    animationDown() {
      document.querySelector("#mainButton").style.transform = "scale(1)";
    },
    startCount() {
      setInterval(this.primaryButton, this.interval);
    },
    stopCount() {
      clearInterval(this.primaryButton);
    },
    restart() {
      localStorage.removeItem("interval", JSON.stringify(this.interval));
      localStorage.removeItem("multiplier", JSON.stringify(this.multiplier));
      localStorage.removeItem("total", JSON.stringify(this.totalDisplay));
      alert("We're going back to the staaart");
      this.totalDisplay = 0;
      this.multiplier = 1;
      this.interval = 5000;
      this.multiplierCost = 10;
      this.intervalCost = 10;
    },

    // better functionality
    upgradeSpeed() {
      if (this.totalDisplay >= this.intervalCost) {
        this.totalDisplay = this.totalDisplay - this.intervalCost;
        this.interval = this.interval / 2;
        this.intervalCost = this.intervalCost * 5;
        this.stopCount();
        this.startCount();
      }
    },
    upgradeMultiplier() {
      if (this.totalDisplay >= this.multiplierCost) {
        this.totalDisplay = this.totalDisplay - this.multiplierCost;
        this.multiplier = this.multiplier * 2;
        this.multiplierCost = this.multiplierCost * 5;
      }
    },
    saveProgress() {
      localStorage.setItem("interval", JSON.stringify(this.interval));
      localStorage.setItem("multiplier", JSON.stringify(this.multiplier));
      localStorage.setItem("total", JSON.stringify(this.totalDisplay));
      alert("progress saved");
    },
    getLocalStorage() {
      if (JSON.parse(localStorage.getItem("interval")) !== null) {
        this.interval = JSON.parse(localStorage.getItem("interval"));
      }
      if (JSON.parse(localStorage.getItem("multiplier")) !== null) {
        this.multiplier = JSON.parse(localStorage.getItem("multiplier"));
      }
      if (JSON.parse(localStorage.getItem("total")) !== null) {
        this.totalDisplay = JSON.parse(localStorage.getItem("total"));
      }
    },

    // Old functionality

    firstMultiplier() {
      let cost = 100;
      let firstMultiplier = 10;
      if (this.totalDisplay >= cost && this.multiplier < firstMultiplier) {
        this.multiplier = firstMultiplier;
        this.totalDisplay = this.totalDisplay - cost;
      }
    },
    secondMultiplier() {
      let cost = 1000;
      let secondMultiplier = 100;
      if (this.totalDisplay >= cost && this.multiplier < secondMultiplier) {
        this.multiplier = secondMultiplier;
        this.totalDisplay = this.totalDisplay - cost;
      }
    },
    thirdMultiplier() {
      let cost = 50000;
      let thirdMultiplier = 500;
      if (this.totalDisplay >= cost && this.multiplier < thirdMultiplier) {
        this.multiplier = thirdMultiplier;
        this.totalDisplay = this.totalDisplay - cost;
      }
    },
    fourthMultiplier() {
      let cost = 250000;
      let fourthMultiplier = 1000;
      if (this.totalDisplay >= cost && this.multiplier < fourthMultiplier) {
        this.multiplier = fourthMultiplier;
        this.totalDisplay = this.totalDisplay - cost;
      }
    },
    fifthMultiplier() {
      let cost = 1000000;
      let fifthMultiplier = 2500;
      if (this.totalDisplay >= cost && this.multiplier < fifthMultiplier) {
        this.multiplier = fifthMultiplier;
        this.totalDisplay = this.totalDisplay - cost;
      }
    },
  },
  mounted() {
    this.getLocalStorage();
    // localStorage.clear();
    this.startCount();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
template {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0;
  padding: 0;
}
p {
  font-size: 10rem;
  margin: 0;
}
button {
  font-size: 3rem;
  display: block;
  margin: 0.5rem;
  background: rgb(62, 168, 35);
  border-radius: 10px;
}
button:hover {
  cursor: pointer;
  background: rgb(50, 134, 29);
}
button p {
  margin: 0.5rem 4rem;
  padding: 1rem;
  font-size: 4rem;
  border-radius: 50%;
  border: 1px solid rgb(1, 63, 1);
}
button::active {
  transform: scale(1.25);
}
</style>
