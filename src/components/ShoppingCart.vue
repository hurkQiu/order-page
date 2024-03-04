<template>
  <div class="container">
    <div style="display: flex; justify-content: center">
      <div class="scroll-area" style="height: 720px">
        <div class="content">
          <div class="previous-step" @click="previousStep">
            <div class="arrow-left"></div>
          </div>
          <div style="background-color: whitesmoke; width: 650px">
            <div style="display: flex">
              <div
                v-for="(item, index) in titles"
                :key="index"
                class="title"
                :style="{ width: item.width }"
              >
                {{ item.title }}
              </div>
            </div>
            <div
              :v-if="orderItems.length > 0"
              v-for="(item, index) in orderItems"
              v-bind:key="index"
              :style="getStyleObject(index)"
              style="margin-left: 10px"
            >
              <div class="product">
                <img :src="`/image/num${item.id}.jpg`" />
                <div style="margin-left: 10px; width: 30%">
                  {{ item.name }}
                </div>
                <div style="width: 35%; text-align: right">
                  {{ item.quantity }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="order-button" style="pointer-events: auto" @click="checkOrder">
      <span>確認送出</span>
    </div>
  </div>
</template>

<script>
export default {
  props: ["orderItems"],
  data() {
    return {
      titles: [
        { title: "內容", width: "10%" },
        { title: "數量", width: "70%" },
      ],
    };
  },
  emits: ["cart-previous-step", "check-order"],
  methods: {
    getStyleObject(index) {
      return {
        borderBottom: index < this.orderItems.length - 1 ? "1px solid" : "none",
      };
    },
    previousStep() {
      this.$emit("cart-previous-step");
    },
    checkOrder() {
      this.$emit("check-order");
    },
  },
};
</script>
<style scoped>
.title {
  text-align: right;
  padding: 10px;
  font-weight: bold;
  font-size: 17pt;
}
</style>
