<template>
  <body>
    <header>
      <h1>Monster Slayer</h1>
    </header>
    <div id="app">
      <section id="monster" class="container">
        <h2>Monster Health</h2>
        <div class="healthbar">
          <div class="healthbar__value" :style="monsterBarStyles"></div>
          <!-- will make monsterHealth a value between 0-100% -->
        </div>
      </section>
      <section id="player" class="container">
        <h2>Your Health</h2>
        <div class="healthbar">
          <div class="healthbar__value" :style="playerBarStyles"></div>
        </div>

        <section class="container" v-if="winner">
          <h2>Game Over!</h2>
          <h3 v-if="winner === 'monster'">You lost!</h3>
          <h3 v-else-if="winner === 'player'">You won!!!</h3>
          <h3 v-else>It's a draw!</h3>
          <button @click="startGame">Start New Game</button>
        </section>
      </section>
      <section id="controls">
        <button @click="attackMonster">ATTACK</button>
        <button :disabled="mayUseSpecialAttack" @click="specialAttackMonster">
          SPECIAL ATTACK
        </button>
        <button @click="healPlayer">HEAL</button>
        <button @click="surrender">SURRENDER</button>
      </section>
      <section id="log" class="container">
        <h2>Battle Log</h2>
        <ul>
          <li v-for="logMessage in logMessages" :key="logMessage">
            <span
              :class="{
                'log--player': logMessage.actionBy === 'player',
                'log--monster': logMessage.actionBy === 'monster',
              }"
              >{{
                logMessage.actionBy === 'player' ? 'Player' : 'Monster'
              }}</span
            >
            <span v-if="logMessage.actionType === 'heal'">
              heals theirself for
              <span class="log--heal">{{ logMessage.actionValue }}</span></span
            >
            <span v-else>
              attacks and deals
              <span class="log--damage">{{ logMessage.actionValue }}</span>
            </span>
          </li>
        </ul>
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
      currentRound: 0,
      winner: null,
      logMessages: [],
    };
  },
  computed: {
    monsterBarStyles() {
      // if the monster bar is below 0, show width as 0%
      // otherwise return monsterHealth width
      if (this.monsterHealth < 0) {
        return { width: '0%' };
      }
      return { width: this.monsterHealth + '%' };
    },
    playerBarStyles() {
      if (this.playerHealth < 0) {
        return { width: '0%' };
      }
      return { width: this.playerHealth + '%' };
    },
    mayUseSpecialAttack() {
      // Every 3 rounds, 'special attack' button is enabled (in <template>); when not divisible by 3
      return this.currentRound % 3 !== 0;
    },
  },
  watch: {
    playerHealth(value) {
      if (value <= 0 && this.monsterHealth <= 0) {
        // draw
        this.winner = 'draw';
      } else if (value <= 0) {
        // Player lost
        this.winner = 'monster';
      }
    },
    monsterHealth(value) {
      if (value <= 0 && this.playerHealth <= 0) {
        // draw
        this.winner = 'draw';
      } else if (value <= 0) {
        // Monster lost
        this.winner = 'player';
      }
    },
  },
  methods: {
    // getRandomValue() defined in methods, then called by methods using 'this.'
    getRandomValue(min, max) {
      return Math.floor(Math.random() * (max - min)) * min;
    },
    startGame() {
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.winner = null;
      this.currentRound = 0;
      this.logMessages = [];
    },
    attackMonster() {
      this.currentRound++;
      const attackValue = this.getRandomValue(5, 12);
      this.monsterHealth -= attackValue;
      this.addLogMessage('player', 'attack', attackValue);
      this.attackPlayer();
    },
    attackPlayer() {
      const attackValue = this.getRandomValue(8, 15);
      this.playerHealth -= attackValue;
      this.addLogMessage('monster', 'attack', attackValue);
    },
    specialAttackMonster() {
      this.currentRound++;
      const attackValue = this.getRandomValue(10, 25);
      this.monsterHealth -= attackValue;
      this.addLogMessage('player', 'attack', attackValue);
      this.attackPlayer();
    },
    healPlayer() {
      this.currentRound++;
      const healValue = this.getRandomValue(8, 20);
      // Starts with 100% health, can't heal ourselves past 100%
      if (this.playerHealth + healValue > 100) {
        this.playerHealth = 100;
      } else {
        this.playerHealth += healValue;
      }
      this.addLogMessage('player', 'heal', healValue);
      // After healing ourselves, the monster will attack the player
      this.attackPlayer();
    },
    surrender() {
      this.winner = 'monster';
    },
    addLogMessage(who, what, value) {
      this.logMessages.unshift({
        actionBy: who,
        actionType: what,
        actionValue: value,
      });
    },
  },
};
</script>

<style src="./styles.css" />
