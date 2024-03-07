<template>
  <div class="container">
    <div class="type-Area">
      <button
        v-for="(button, index) in typeList"
        :key="index"
        :class="['type-button', { active: activeIndex === index }]"
        @click="scrollToType(index)"
      >
        {{ button.text }}
      </button>
      <div
        class="record-button"
        @click="showRecordPage"
        :style="recordClickStyle"
      >
        <span>訂單紀錄</span>
      </div>
    </div>
    <div style="display: flex; justify-content: center">
      <div class="scroll-area">
        <div class="content">
          <ProductList
            ref="productListRef"
            @item-quantity-change="setShoppingCart"
            :orderItemList="shoppingCart"
          />
        </div>
      </div>
    </div>
    <div class="order-button" @click="showCartPage" :style="addClickStyle">
      <span>加入購物車</span>
    </div>
  </div>
</template>

<script setup>
import {
  computed,
  onBeforeUnmount,
  onMounted,
  ref,
  defineEmits,
  inject,
  defineProps,
} from "vue";
import ProductList from "./ProductList.vue";
import ShoppingItem from "../dataType/DataType";
const props = defineProps({ recordCanClick: Boolean });
const emits = defineEmits(["add-to-cart", "show-record-page"]);
const productListRef = ref(null);
const activeIndex = ref(0);
const shoppingCart = ref(inject("reset"));
const typeList = ref([
  { text: "肉類" },
  { text: "海鮮類" },
  { text: "蔬菜類" },
]);
const addClickStyle = computed(() => ({
  "pointer-events": shoppingCart.value.length <= 0 ? "none" : "auto",
}));
const recordClickStyle = computed(() => ({
  "pointer-events": props.recordCanClick === false ? "none" : "auto",
}));
const showCartPage = () => {
  emits("add-to-cart", shoppingCart.value);
};
const showRecordPage = () => {
  emits("show-record-page");
};
const setShoppingCart = (data) => {
  const shoppingCartValue = shoppingCart.value;
  const shoppingItem = shoppingCartValue.find((item) => item.id === data.id);
  if (!shoppingItem) {
    const newItem = new ShoppingItem(data.id, data.name);
    newItem.setQuantity(data.quantity);
    shoppingCartValue.push(newItem);
  } else if (data.quantity === 0) {
    const index = shoppingCartValue.indexOf(shoppingItem);
    shoppingCartValue.splice(index, 1);
  } else {
    shoppingItem.setQuantity(data.quantity);
  }
  shoppingCart.value = shoppingCartValue;
};

const toggleActive = (index) => {
  if (activeIndex.value != index) {
    activeIndex.value = index;
  }
};
const scrollToType = (index) => {
  let pos = 0;
  toggleActive(index);
  switch (index) {
    case 0:
      pos = 0;
      break;
    case 1:
      pos = productListRef.value.meatItems.length * 100;
      break;
    case 2:
      pos =
        (productListRef.value.meatItems.length +
          productListRef.value.seafoodItems.length) *
        100;
      break;
    default:
      break;
  }
  window.scrollTo({
    top: pos,
    behavior: "smooth",
  });
};
const checkPosition = () => {
  const scrollTop = window.scrollY || document.documentElement.scrollTop;
  const meatHeight = productListRef.value.meatItems.length * 100;
  const seafoodHeight = productListRef.value.seafoodItems.length * 100;
  const oneThirdScreenHeight = window.innerHeight / 3;
  if (scrollTop < meatHeight - oneThirdScreenHeight) toggleActive(0);
  else if (
    scrollTop > meatHeight - oneThirdScreenHeight &&
    scrollTop < meatHeight + seafoodHeight - oneThirdScreenHeight
  )
    toggleActive(1);
  else toggleActive(2);
};

onMounted(() => {
  shoppingCart.value = [];
  window.addEventListener("scroll", checkPosition);
});
onBeforeUnmount(() => {
  window.removeEventListener("scroll", checkPosition);
});
</script>
<style scoped>
.type-Area {
  width: 600px;
  display: flex;
  justify-content: space-evenly;
  align-items: stretch;
  margin: auto;
  position: sticky;
  top: 0;
  z-index: 1000;
}

.type-button {
  width: 33%;
  height: 30px;
  background-color: white;
  border: 0;
  border-radius: 16px;
  background-color: lightgrey;
}
.type-button:hover {
  background-color: whitesmoke;
  transform: scale(1.05);
  border: 2px solid;
  font-weight: bold;
}
.type-button.active,
.type-button:active {
  background-color: whitesmoke;
  transform: scale(1);
  font-weight: bold;
  border: 2px solid;
}
@media (max-width: 720px){
  .type-Area {
    width: 720px;
  }
}
@media (min-width: 360px) and (max-width: 720px){
  .type-Area {
    width: 100%;
  }
}
@media (max-width: 360px){
  .type-Area {
    width: 100%;
  }
}
</style>
