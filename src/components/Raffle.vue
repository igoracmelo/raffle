<template>
  <div class="raffle" :class="computedClasses" :style="computedStyle" @click="onClick">
    <span class="number">{{ number }}</span>
    <span v-if="owner">{{ owner.name }}</span>
    <!-- <span v-if="timeoutToUnlock">{{ timeoutToUnlock }} seconds</span> -->
  </div>
</template>

<script>
export default {
  props: {
    number: Number,
    owner: Object,
    // value: Boolean
    isSelected: Boolean
    // intervalToUnlock: Number
  },

  data() {
    return {
      colors: [
        '#be96ef',
        '#dd92d3',
        '#dabf88',
        '#e8bdb4',
        '#ade6b1',
        '#addce6',
        '#e2a3ae',
        '#91d57e',
        '#97cff0',
        '#f0d996',
      ],

      // isSelected: false
      // timeoutId: null,
      // number: 1,
      // owner: { name: 'Giselle Cristelle' },
      // timeoutToUnlock: 0,
      // status: 'BOUGHT' // computed?
    }
  },

  methods: {
    onClick () {
      if (this.status === 'AVAILABLE') {
        this.$emit('update:isSelected', !this.isSelected)
      }
    }
    // startTimeout() {
    //   if (!this.timeoutToUnlock) {
    //     return
    //   }
    //   this.timeoutId = setInterval(() => {
    //     if (this.timeoutToUnlock <= 0) {
    //       this.status = 'AVAILABLE'
    //       return
    //     }
    //     this.status = 'LOCKED'
    //     this.timeoutToUnlock -= 1
    //   }, 1000)
    // }
  },

  computed: {
    status () {
      if (this.owner) {
        return 'BOUGHT'
      }
        // if (this.intervalToUnlock) {
        //   return 'LOCKED'
        // }
      return 'AVAILABLE'
    },

    computedStyle () {
      // if (true !== 'true')
      return { backgroundColor: this.colors[this.number % 10] }
      // if (!this.status === 'BOUGHT') {
      // }

      // return {}
    },

    computedClasses () {
      return {
        bought: this.status === 'BOUGHT',
        selected: this.isSelected
      }
    },

    // isSelected () {
    //   // console.log(this.value)
    //   return this.value || false
    // }

    // isAvailable() {
    //   return this.status === 'AVAILABLE'
    // },

    // isLocked() {
    //   return this.status === 'LOCKED'
    // },

    // isBought() {
    //   return this.status === 'BOUGHT'
    // }
  }
}
</script>

<style scoped>
.raffle {
  /* width: 150px; */
  height: 120px;
  padding: 15px;
  box-shadow: 0 2px 7px #0003;
  display: flex;
  flex-direction: column;
  gap: 10px;
  justify-content: center;
  align-items: center;
  /* background-color: #0f02; */
  cursor: pointer;
  border-radius: 5px;
  /* text-align: center; */
}


/* .raffle.available {
  background-color: #0f02;
} */
/* .raffle.locked {
  opacity: 0.5;
  background-color: #ff03;
} */
.raffle.selected {
  box-shadow: 0 0 4px 3px  #000a;
}

.raffle.bought {
  opacity: 0.4;
  background-color: #0005 !important;
  cursor: not-allowed
}

.raffle .number {
  font-size: 40px;
  font-weight: bold;
}
</style>
