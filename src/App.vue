<template>
  <ShoppingPage
    @add-to-cart="handleOrderItems"
    @show-record-page="showRecordPage"
    v-show="showShoppingPage()"
    :recordCanClick="recordCanClick"
  />
  <div v-show="isShowOrderCart" class="page">
    <ShoppingCart
      v-show="isShowOrderCart"
      :orderItems="orderItems"
      @cart-previous-step="cartPreviousStep"
      @check-order="checkOrder"
    />
  </div>
  <div v-show="isShowRecord" class="page">
    <OrderRecordVue
      v-show="isShowRecord"
      :orderItems="recordItems"
      @record-previous-step="recordPreviousStep"
    />
  </div>
</template>

<script setup>
import { provide, ref } from "vue";
import ShoppingPage from "./components/ShoppingPage.vue";
import ShoppingCart from "./components/ShoppingCart.vue";
import OrderRecordVue from "./components/OrderRecord.vue";
const isShowOrderCart = ref(false);
const isShowRecord = ref(false);
const recordCanClick = ref(false);
const orderItems = ref([]);
const recordItems = ref([]);
const handleOrderItems = (data) => {
  orderItems.value = data;
  isShowOrderCart.value = true;
};
const cartPreviousStep = () => {
  isShowOrderCart.value = false;
};
const checkOrder = () => {
  recordCanClick.value = true;
  isShowOrderCart.value = false;
  orderItems.value.forEach((item) => {
    recordItems.value.push(item);
  });
  orderItems.value = [];
};
const showShoppingPage = () => {
  return isShowOrderCart.value === false && isShowRecord.value === false;
};
const showRecordPage = () => {
  isShowRecord.value = true;
};
const recordPreviousStep = () => {
  isShowRecord.value = false;
};
provide("reset", orderItems);
</script>
