<template>
  <button @click="subQuantity">-</button>
  <input :value="itemQuantity" min="min" max="max" type="text" disabled />
  <button @click="addQuantity">+</button>
</template>

<script>
import { ref } from "vue";

export default {
  props: ["itemQuantity", "id", "name"],
  emits: ["item-quantity-change"],
  data() {
    return {
      max: 10,
      min: 0,
    };
  },
  setup() {
    const quantity = ref(0);
    return { quantity };
  },
  methods: {
    addQuantity() {
      if (this.quantity < this.max) {
        this.quantity++;
        this.sendData();
      }
    },
    subQuantity() {
      if (this.quantity > this.min) {
        this.quantity--;
        this.sendData();
      }
    },
    sendData() {
      this.$emit("item-quantity-change", {
        id: this.id,
        quantity: this.quantity,
        name: this.name,
      });
    },
  },
  watch: {
    itemQuantity: function (newQuantity) {
      if (newQuantity !== this.quantity) this.quantity = 0;
    },
  },
};
</script>

<style scoped>
input {
  border: none;
  text-align: center;
  width: 10%;
  font-size: 16px;
  min-width: 23px;
}
button {
  width: 15%;
  height: 15%;
  border-radius: 50%;
  font-size: 16px;
  min-width: 30px;
}
</style>
