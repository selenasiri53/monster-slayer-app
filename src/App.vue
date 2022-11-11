<template>
  <body>
    <header>
      <h1>Monster Slayer</h1>
    </header>
    <div id="app">
      <section id="monster" class="container">
        <h2>Monster Health</h2>
        <div class="healthbar">
          <div class="healthbar__value"></div>
        </div>
      </section>
      <section id="player" class="container">
        <h2>Your Health</h2>
        <div class="healthbar">
          <div class="healthbar__value"></div>
        </div>
      </section>
      <section id="controls">
        <button>ATTACK</button>
        <button>SPECIAL ATTACK</button>
        <button>HEAL</button>
        <button>SURRENDER</button>
      </section>
      <section id="log" class="container">
        <h2>Battle Log</h2>
        <ul></ul>
      </section>
    </div>
  </body>
</template>

<script>
export default {
  name: 'App',
  components: {},
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
    };
  },
  methods: {
    attackMonster() {
      // Generate a random number between 5 and 12
      const attackValue = Math.floor(Math.random() * (12 - 5)) + 5;
      this.monsterHealth -= attackValue;
    },
    attackPlayer() {
      const attackValue = Math.floor(Math.random() * (15 - 8)) + 8;
      this.playerHealth -= attackValue;
    },
  },
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
</style>
