<template>
  <div id="app">
    <h1>HIGGPAD</h1>
    <p>Don't let your dreams be dreams</p>
    <section class="container" id="pad-container">
      <div class="pad">
        <img
          src="https://cdn4.iconfinder.com/data/icons/musical-instruments-32/160/bass-drum-512.png"
          class="icon center"
        />
        <h1 class="letter center">A</h1>
      </div>
      <div class="pad">
        <img
          src="https://static.thenounproject.com/png/252979-200.png"
          class="icon center"
        />
        <h1 class="letter center">S</h1>
      </div>
      <div class="pad">
        <img
          src="https://static.thenounproject.com/png/252979-200.png"
          class="icon center"
        />
        <h1 class="letter center">D</h1>
      </div>
      <div class="pad">
        <img
          src="https://image.flaticon.com/icons/png/512/1755/1755661.png"
          class="icon center"
        />
        <h1 class="letter center">F</h1>
      </div>
      <div class="pad">
        <img
          src="https://cdn1.iconfinder.com/data/icons/editing/60/cell-12-2-480.png"
          class="icon center"
        />
        <h1 class="letter center">G</h1>
      </div>
    </section>
  </div>
</template>

<script>
import { Sampler } from "tone";
// import samples
import kick from "./assets/kick.mp3"
import snare from "./assets/NM - Dumbo Snare.wav";
import snare2 from "./assets/NM - Fendi Snare.wav";
import hihat1 from "./assets/NM - Duality HH.wav";
import eightOEight from "./assets/NM - Flock 808.wav";
// set up functionality
export default {
  name: "App",
  components: {},
  created() {
    const component = this;
    this.sampler = new Sampler(
      {
        C1: kick,
        D1: snare,
        E1: snare2,
        F1: hihat1,
        G1: eightOEight
      },
      {
        onLoad: () => {
          this.isLoaded = true;
        }
      }
    ).toDestination();

    component.keydownHandler = function(event) {
      component.playSound(event.key.toLowerCase());
    };

    window.addEventListener("keydown", this.keydownHandler);
  },
  beforeDestroy() {
    window.removeEventListener("keydown", this.keydownHandler);
  },
  methods: {
    playA() {
      this.sampler.triggerAttack("C1");
    },
    playS() {
      this.sampler.triggerAttack("D1");
    },
    getNoteFromKey(key) {
      const options = {
        a: "C1",
        s: "D1",
        d: "E1",
        f: "F1",
        g: "G1"
      };
      return key in options ? options[key] : null;
    },
    playSound(key) {
      const note = this.getNoteFromKey(key);
      if (note) this.sampler.triggerAttack(note);
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

/* container styling */
.container {
  display: flex;
  flex-wrap: wrap;
  padding: 0px;
  justify-content: center;
}

/* pad syling */
.pad {
  background-color: grey;
  width: 200px;
  height: 200px;
  margin: 10px;
  position: relative;
}

.center {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.icon {
  width: 100px;
  height: 100px;
  background-color: white;
}

.letter {
  color: white;
  top: 100px;
  background-color: black;
  width: 40px;
}
</style>
