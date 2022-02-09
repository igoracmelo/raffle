<template>
  <div class="container">
    <Cursor
      v-for="cursor of cursors"
      :key="cursor.id"
      :x="cursor.x"
      :y="cursor.y"
    />
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
import Cursor from "./components/Cursor.vue";
// import io from "socket.io-client";

// const socket = io("http://localhost:3000/");

export default {
  name: "App",
  components: {
    Raffle,
    Buyer,
    Cursor,
  },

  created() {
    window.onmousemove = (e) => {
      socket.emit("mousemove", [e.clientX, e.clientY]);
    };

    socket.on("join", (id) => {
      this.cursors.push({ id, x: 100, y: 150 });
    });

    // window.onmousemove = (e) => {
    //   socket.emit("mousemove", [e.clientX, e.clientY]);
    // };

    // socket.on("join", (id) => {
    //   this.cursors.push({ id, x: 100, y: 150 });
    // });

    // socket.on("mousemove", (id, [x, y]) => {
    //   let cursor = this.cursors.find((cursor) => cursor.id === id);

    //   if (!cursor) {
    //     this.cursors.push({ id, x, y });
    //     return;
    //   }

    //   cursor.x = x;
    //   cursor.y = y;
    // });

    // socket.on("left", (id) => {
    //   this.cursors = this.cursors.filter((c) => c.id !== id);
    // });
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
      cursors: [],
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
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 10px;
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
