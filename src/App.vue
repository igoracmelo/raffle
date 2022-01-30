<template>
  <div class="container">
    <div class="header">
      <input
        v-model="pricePerUnit"
        class="input"
        type="text"
        placeholder="Valor unitário"
      />
      <span v-if="selected">
        <b>{{ selected.length }}</b> selecionadas = R$
        <b>{{ totalPrice.toFixed(2) }}</b>
      </span>
      <input v-model="ownerName" class="input" type="text" placeholder="Nome" />
      <button class="btn" :disabled="!selected.length" @click="assign">
        Atribuir
      </button>
    </div>
    <div class="raffles">
      <Raffle
        v-for="raffle in raffles"
        :key="raffle.number"
        :number="raffle.number"
        v-model:isSelected="raffle.isSelected"
        v-model:owner="raffle.owner"
      />
    </div>
    <span class="title" v-if="buyersInfo.length">Compradores</span>
    <div class="buyers">
      <Buyer
        v-for="[name, count] of buyersInfo"
        :key="name"
        :name="name"
        :count="count"
        :pricePerUnit="pricePerUnit"
      />
    </div>
  </div>
</template>

<script>
import Raffle from "./components/Raffle.vue";
import Buyer from "./components/Buyer.vue";

export default {
  name: "App",
  components: {
    Raffle,
    Buyer,
  },

  data() {
    const raffles = [];

    for (let i = 1; i <= 50; i++) {
      raffles.push({
        number: i,
        isSelected: false,
      });
    }

    return {
      raffles,
      pricePerUnit: "",
      ownerName: "",
    };
  },

  methods: {
    assign() {
      this.selected.forEach((raffle) => {
        raffle.owner = { name: this.ownerName };
        raffle.isSelected = false;
      });
    },
  },

  computed: {
    selected() {
      return this.raffles.filter((raffle) => raffle.isSelected);
    },

    buyersInfo() {
      let counter = {};

      this.raffles
        .filter((raffle) => raffle.owner)
        .map((raffle) => raffle.owner.name)
        .forEach((name) => {
          counter[name] = counter[name] || 0;
          counter[name] += 1;
        });

      return Object.entries(counter).sort((a, b) => b[1] - a[1]);
    },

    totalPrice() {
      return this.selected.length * this.pricePerUnit;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: grid;
  place-items: center;
}

.container {
  width: 1200px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.raffles {
  display: grid;
  grid-gap: 7px;
  grid-template-columns: repeat(10, 110px);
}

.header {
  display: flex;
  height: 70px;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  gap: 30px;
}

.title {
  width: 100%;
  text-align: center;
  font-size: 25px;
}

.buyers {
  font-size: 17px;
  display: grid;
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
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
