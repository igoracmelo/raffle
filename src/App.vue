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
        <b>{{ selected.length }}</b> rifa(s) = R$
        <b>{{ totalPrice.toFixed(2) }}</b>
      </span>
      <input v-model="ownerName" class="input" type="text" placeholder="Nome" />
      <button class="btn" :disabled="!selected.length || !ownerName" @click="assign">
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
    let raffles = localStorage.getItem("raffles");

    if (raffles) {
      const parsedRaffles = JSON.parse(raffles)
      if (Array.isArray(parsedRaffles)) {
        this.raffles = parsedRaffles
      }
    }

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

  watch: {
    raffles: {
      deep: true,
      handler (val) {
        console.log(val);
        localStorage.setItem("raffles", JSON.stringify(val));
      }
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body, #app {
  width: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: grid;
  place-items: center;
}

.container {
  padding: 20px;
  width: min(950px, 100%);
  display: flex;
  flex-direction: column;
  gap: 50px;
}

.raffles {
  width: 100%;
  display: grid;
  grid-gap: 7px;
  grid-template-columns: repeat(auto-fit, minmax(80px, 1fr));
  grid-auto-rows: 80px;
}

.header {
  display: flex;
  width: 100%;
  justify-content: center;
  font-size: 20px;
  gap: 30px;
}

.header > * {
  text-align: center;
}

@media screen and (max-width: 950px) {
  .header {
    flex-wrap: wrap;
  }

  .header > * {
    flex: 1 0 40%;
  }
}

@media screen and (max-width: 450px) {
  .header > * {
    flex: 1 0 100%;
  }
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
  min-width: 100px;
}

.btn {
  min-width: 80px;
  font-size: 16px;
  font-weight: bold;
  color: white;
  background-color: #5d7;
  padding: 10px 30px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
}

.btn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}
</style>
