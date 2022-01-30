/* eslint-disable no-debugger */
<template>
  <div>
    <div class="header">
      <span>Selecione rifas para poder atribuir a alguém.</span>
      <span v-if="selected.length">{{ selected.length }} selecionadas.</span>
    </div>
    <div class="raffles">
      <Raffle v-for="raffle in raffles"
        v-model="raffle.isSelected"
        :key="raffle.number"
        :number="raffle.number"
        :owner="raffle.owner"
      />
      <!-- <Raffle :number="1" />
      <Raffle :number="2" :owner="{ name: 'zezin' }" />
      <Raffle :number="3" :owner="{ name: 'mariazinha' }" />
      <Raffle :number="4" /> -->
    </div>
  </div>
</template>

<script>
import Raffle from './components/Raffle.vue'

export default {
  name: 'App',
  components: {
    Raffle
  },

  data () {
    const raffles = []

    for (let i = 1; i <= 50; i++) {
      const owner = Math.random() > 0.8 ? { name: 'rato borrachudo' } : null

      raffles.push({
        number: i,
        owner,
        isSelected: true
      })
    }

    return {
      raffles,
    }
  },

  computed: {
    selected () {
      // eslint-disable-next-line no-debugger
      // debugger
      return this.raffles.filter(raffle => raffle.isSelected)
    }
  }
}
</script>

<style>
/* body {
  } */

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.raffles {
  /* display: flex;
  gap: 15px; */

  display: grid;
  grid-gap: 10px;
  grid-template-columns: repeat(10, 1fr);
  padding: 15px;
}

.header {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 30px;
}
</style>
