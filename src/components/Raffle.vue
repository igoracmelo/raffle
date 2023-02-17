<template>
  <div
    class="raffle"
    :class="computedClasses"
    :style="computedStyle"
    @click="onClick"
  >
    <span class="number">{{ number }}</span>
    <span v-if="owner">{{ owner.name }}</span>
  </div>
</template>

<script>
export default {
  props: {
    number: Number,
    owner: Object,
    isSelected: Boolean
  },

  data () {
    return {
      touchCount: 0,
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
        '#f0d996'
      ]
    }
  },

  methods: {
    onClick () {
      if (this.status === 'AVAILABLE') {
        this.$emit('update:isSelected', !this.isSelected)
      } else if (this.status === 'BOUGHT') {
        setTimeout(() => {
          this.touchCount = 0
        }, 2000)
        this.touchCount++
        if (this.touchCount >= 2) {
          this.onDoubleClick()
        }
      }
    },

    onDoubleClick () {
      if (this.status === 'BOUGHT') {
        this.$emit('update:isSelected', false)
        this.$emit('update:owner', null)
      }
    }
  },

  computed: {
    status () {
      if (this.owner) {
        return 'BOUGHT'
      }
      return 'AVAILABLE'
    },

    computedStyle () {
      return { backgroundColor: this.colors[this.number % 10] }
    },

    computedClasses () {
      return {
        bought: this.status === 'BOUGHT',
        selected: this.isSelected
      }
    }
  }
}
</script>

<style scoped>
.raffle {
  padding: 15px;
  box-shadow: 0 2px 7px #0003;
  display: flex;
  flex-direction: column;
  gap: 10px;
  justify-content: center;
  cursor: pointer;
  border-radius: 5px;
  user-select: none;
  text-align: center;
}

.raffle.selected {
  box-shadow: 0 0 4px 3px #000a;
}

.raffle.bought {
  opacity: 0.4;
  background-color: #0005 !important;
  cursor: not-allowed;
}

.raffle .number {
  font-size: 25px;
  font-weight: bold;
}
</style>
