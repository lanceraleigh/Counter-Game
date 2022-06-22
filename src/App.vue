<template>
  <img src="./assets/money-background.jpg" class="background" />
  <HelloWorld />
</template>

<script>
import HelloWorldVue from "./components/HelloWorld.vue";
export default {
  name: "App",
  components: {
    HelloWorld: HelloWorldVue,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #97d495;
  width: 100%;
  height: 100%;
  background: rgba(40, 40, 40, 0.8);
  position: absolute;
  top: 0;
  left: 0;
  overflow: hidden;
}
.background {
  position: absolute;
  top: -400px;
  left: 0;
  height: 200%;
  z-index: -1;
  animation-name: sliding;
  animation-duration: 60s;
  animation-iteration-count: infinite;
  animation-timing-function: ease-in-out;
}

@keyframes sliding {
  0% {
    transform: translate(0);
  }
  50% {
    transform: translatey(15%);
  }
  100% {
    transform: translate(0);
  }
}
</style>
