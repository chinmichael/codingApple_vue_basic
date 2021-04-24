<template>
  <div class="product-list">
    <!--재사용성을 위해 축약컴포넌트가 아닌 사용하는 곳에서 v-for-->
    <img :src="oneroom_list.image" class="room-img" />
    <!-- <h4
      @click="
        modal_status = true;
        modal_index = i;
      "
      :style="style1"
    > -->
    <!-- <h4 style="color: blue" v-on:click="$emit('openModal', index)"> -->
    <h4 style="color: blue" v-on:click="openModal()">
      <!--index주고 받지 않아도 데이터에 아이디가 있다...-->
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
    index: Number,
  },
  methods: {
    openModal() {
      this.$emit("openModal", this.index);
    },
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

<!--4/24 :  custom event
            하위컴포넌트에서 상위컴포넌트 데이터수정 (state 전환이 가능)

강의에서 안된다고 뒤에 나왔지만 먼저 시도해보니
props를 수정시키는건 쌉안됨 (props는 read only니까)
data를 박고 props를 넣어서 data를 바꾸는것도 쌉안됨

App.vue의 Card에 이벤트를 바인딩하면
이벤트 버블링(HTML요소를 클릭시 그것이 감싸는 요소 전부 클릭된거) 해당 요소 전부 달리니 표적설정이....

custom event : 상위 컴포넌트 데이터 변경이 안되기에 상위컴포넌트에 메시지만 보냄
발신측 : @onclick="$emit('메시지명', 데이터)" $~는 제이쿼리 뿐 아니라 뷰에서도 쓰는 특별 변수
수신측 : 해당 컴포넌트 태그에 @메시지명="자바스크립트" : 해당 메시지가 왔을때 해당 JS 실행
       : 참고로 수신측 @메시지명="$event"는 $emit에서 데이터가 있을 경우 데이터를 저기 담아 보냄

물론 onclick 이벤트에 들어가기에 $emit()은 컴포넌트JS의 method:{}영역에 함수로 축약 가능
이때 내부요소의 것을 쓰므로 $emit과 데이터변수에 this.을 붙이는걸 매우 유념!!

결국 상위컴포넌트 데이터를 수정하는수밖에 읍따
-->