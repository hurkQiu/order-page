<template>
  <div class="container">
    <div style="display: flex; justify-content: center">
      <div class="scroll-area" style="height: 720px">
        <div class="content">
          <div class="previous-step" @click="handlePreviousStep">
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
                <div style="margin-left: 10px; width: 33%">
                  {{ item.name }}
                </div>
                <div style="width: 33%; text-align: right">
                  {{ item.quantity }}
                </div>
                <div style="width: 22%; text-align: right">
                  {{ getOrderState(item.state) }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["orderItems"],
  emits: ["record-previous-step"],
  data() {
    return {
      titles: [
        { title: "訂單內容", width: "10%" },
        { title: "數量", width: "60%" },
        { title: "狀態", width: "15%" },
        { title: "", width: "5%" },
      ],
    };
  },
  methods: {
    getStyleObject(index) {
      return {
        borderBottom: index < this.orderItems.length - 1 ? "1px solid" : "none",
      };
    },
    getOrderState(state) {
      return state === false ? "未送達" : "已送達";
    },
    handlePreviousStep() {
      this.$emit("record-previous-step");
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
