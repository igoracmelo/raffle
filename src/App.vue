/* eslint-disable no-debugger */
<template>
  <div>
    <div class="header">
      <!-- <span>Selecione rifas para poder atribuir a alguém.</span> -->
      <input v-model="pricePerUnit" class="input" type="text" placeholder="Valor unitário" />
      <span v-if="selected">
        <b>{{ selected.length }}</b> selecionadas = R$ <b>{{ totalPrice.toFixed(2) }}</b>
      </span>
      <input v-model="ownerName" class="input" type="text" placeholder="Nome" />
      <button class="btn" :disabled="!selected.length" @click="assign">Atribuir</button>
    </div>
    <div class="raffles">
      <Raffle v-for="raffle in raffles"
        :key="raffle.number"
        :number="raffle.number"
        v-model:isSelected="raffle.isSelected"
        v-model:owner="raffle.owner"
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
      // const owner = Math.random() > 0.8 ? { name: 'rato borrachudo' } : null

      raffles.push({
        number: i,
        // owner,
        isSelected: false
      })
    }

    return {
      raffles,
      pricePerUnit: '',
      ownerName: ''
    }
  },

  methods: {
    assign () {
      this.selected.forEach(raffle => {
        raffle.owner = { name: this.ownerName }
        raffle.isSelected = false
      })
    }
  },

  computed: {
    selected () {
      // eslint-disable-next-line no-debugger
      // debugger
      return this.raffles.filter(raffle => raffle.isSelected)
    },

    totalPrice () {
      return this.selected.length * this.pricePerUnit
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
  height: 70px;
  /* flex-direction: column; */
  justify-content: center;
  align-items: center;
  font-size: 25px;
  gap: 30px;
}

.input {
  font-size: 18px;
  padding: 5px 10px;
  border: none;
  border-bottom: 1px solid #0003;
  text-align: center;
  outline: none;
  width: 170px;
}

.btn {
  /* text-transform: uppercase; */
  width: 150px;
  font-size: 16px;
  font-weight: bold;
  color: white;
  background-color: #5d7;
  padding: 10px 10px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
}

.btn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}
</style>
