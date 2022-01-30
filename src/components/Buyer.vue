<template>
  <div class="buyer">
    <div class="name">{{ name }}</div>
    <div class="units">
      <b>{{ count }}</b> unidades
    </div>
    <div class="owes">R$ {{ (count * pricePerUnit).toFixed(2) }}</div>
  </div>
</template>

<script>
export default {
  props: {
    name: String,
    count: Number,
    pricePerUnit: Number,
  },
};
</script>

<style scoped>
.buyer {
  padding: 15px;
  box-shadow: 0 2px 5px 1px #0003;
  display: flex;
  flex-direction: column;
  gap: 6px;
  text-align: center;
  justify-content: center;
  align-items: center;
  flex-basis: 115px;
}

.buyer .name {
  font-size: 18px;
}

.buyer .units {
  font-size: 15px;
}

.buyer .owes {
  font-size: 18px;
  color: #051;
}
</style>