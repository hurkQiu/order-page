<template>
  <div>
    <ul>
      <div class="title">肉類</div>
      <li
        v-for="(item, index) in meatItems"
        :key="item.id"
        :style="getStyleObject(index, item.itemType)"
      >
        <ProductItem
          :item-type="item.itemType"
          :path="item.path"
          :level="item.level"
          :name="item.name"
          :id="item.id"
          :itemQuantity="getItemQuantity(item.id)"
          @item-quantity-change="sendData"
        />
      </li>
    </ul>
    <ul>
      <div class="title">海鮮</div>
      <li
        v-for="(item, index) in seafoodItems"
        :key="item.id"
        :style="getStyleObject(index, item.itemType)"
      >
        <ProductItem
          :item-type="item.itemType"
          :path="item.path"
          :level="item.level"
          :name="item.name"
          :id="item.id"
          :itemQuantity="getItemQuantity(item.id)"
          @item-quantity-change="sendData"
        />
      </li>
    </ul>
    <ul>
      <div class="title">蔬菜</div>
      <li
        v-for="(item, index) in vegetableItems"
        :key="item.id"
        :style="getStyleObject(index, item.itemType)"
      >
        <ProductItem
          :item-type="item.itemType"
          :path="item.path"
          :level="item.level"
          :name="item.name"
          :id="item.id"
          :itemQuantity="getItemQuantity(item.id)"
          @item-quantity-change="sendData"
        />
      </li>
    </ul>
  </div>
</template>
<script>
import axios from "axios";
import ProductItem from "@/components/ProductItem.vue";

export default {
  props: ["orderItemList"],
  data() {
    return {
      meatItems: [],
      seafoodItems: [],
      vegetableItems: [],
      meatSize: 0,
      seafoodSize: 0,
      vegetableSize: 0,
    };
  },
  components: {
    ProductItem,
  },
  emits: ["item-quantity-change"],
  mounted() {
    axios
      .all([
        axios.get("/data/meat.json"),
        axios.get("/data/seafood.json"),
        axios.get("/data/vegetable.json"),
      ])
      .then(
        axios.spread((response1, response2, response3) => {
          this.meatItems = response1.data.items;
          this.seafoodItems = response2.data.items;
          this.vegetableItems = response3.data.items;
          this.meatSize = response1.data.items.length;
          this.seafoodSize = response2.data.items.length;
          this.vegetableSize = response3.data.items.length;
        })
      )
      .catch((error) => {
        console.error("Error loading data:", error);
      });
  },
  methods: {
    getStyleObject(index, type) {
      switch (type) {
        case "1":
          return {
            borderBottom:
              index < this.meatItems.length - 1 ? "1px solid" : "none",
          };
        case "2":
          return {
            borderBottom:
              index < this.seafoodItems.length - 1 ? "1px solid" : "none",
          };
        case "3":
          return {
            borderBottom:
              index < this.vegetableItems.length - 1 ? "1px solid" : "none",
          };
        default:
          break;
      }
    },
    getItemQuantity(id) {
      const shoppingItem = this.orderItemList.find((item) => item.id === id);
      if (!shoppingItem) {
        return 0;
      } else {
        return shoppingItem.quantity;
      }
    },
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

<style scoped>
li {
  display: flex;
  width: 100%;
  height: 100px;
  align-items: center;
  font-weight: bold;
}
ul {
  background-color: white;
}
.title {
  width: 100%;
  font-size: 30px;
  margin: auto;
  font-weight: 900;
  margin-bottom: 5px;
}
</style>
