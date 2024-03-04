<template>
  <div class="product">
    <img v-bind:src="path" />
    <div style="margin-left: 10px">{{ name }}</div>
    <div
      style="
        display: flex;
        margin-left: auto;
        justify-content: flex-end;
        margin-right: 20px;
      "
    >
      <SubAddQuantity
        :id="id"
        :name="name"
        @item-quantity-change="sendData"
        :itemQuantity="itemQuantity"
      />
    </div>
  </div>
</template>

<script>
import SubAddQuantity from "./SubAddQuantity.vue";
export default {
  props: ["path", "name", "itemType", "level", "id", "itemQuantity"],
  emits: ["item-quantity-change"],
  components: {
    SubAddQuantity,
  },
  methods: {
    sendData(data) {
      this.$emit("item-quantity-change", {
        id: data.id,
        quantity: data.quantity,
        name: data.name,
      });
    },
  },
};
</script>
