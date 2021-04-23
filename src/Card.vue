<template>
  <div class="product-list">
    <img :src="oneroom_list.image" class="room-img" />
    <!-- <h4
      @click="
        modal_status = true;
        modal_index = i;
      "
      :style="style1"
    > -->
    <h4 style="color: blue">
      {{ oneroom_list.title }}
    </h4>
    <p>{{ oneroom_list.price }}원</p>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    oneroom_list: Object,
  },
  data() {
    return {};
  },
};
</script>

<style>
div {
  box-sizing: border-box;
}
.room-img {
  width: 100%;
  margin-top: 40px;
}
</style>