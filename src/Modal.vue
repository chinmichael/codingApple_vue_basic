<template>
  <div class="black-bg" v-if="modal_status == true">
    <!-- v-on:click="modal_status = false"
  > -->
    <div class="white-bg">
      <img :src="oneroom[modal_index].image" class="room-modal-img" />
      <h4>{{ oneroom[modal_index].title }}</h4>
      <p>{{ oneroom[modal_index].price }}원</p>
      <p>{{ oneroom[modal_index].content }}</p>
      <!-- <button v-on:click="modal_status = false">close</button> -->
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  props: {
    oneroom: Array, //자료형은 그냥 확인용 정도 (에러가 나진 않는다)
    modal_status: Boolean,
    modal_index: Number,
  },
  data() {
    return {
      //modal_status_in: this.modal_status, //this로 가져온 여기서의 변수임을 세팅함 중요
    };
  },
};
</script>

<style>
div {
  box-sizing: border-box;
}

.black-bg {
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  position: fixed;
  padding: 20px;
  transition: all 1s;
}

.white-bg {
  width: 100%;
  background-color: white;
  border-radius: 8px;
  padding: 20px;
}

.room-modal-img {
  width: 300px;
  margin-top: 40px;
}
</style>

<!-- 4/23 : props

너무 많이 컴포넌트를 만들면 코드가 필연적으로 구조가 복잡해짐>>데이터 문제가 발생 쉬움
예를 들어 모달을 컴포넌트한다고 하면
modal이 상위 컴포넌트에서 가져온 데이터바인딩에 이래저래 문제가 터지기 쉬움

따라서 반복되는 UI에 사용하는것이 좋음

근데 모달을 컴포넌트로 만들면서 데이터 바인딩 문제를 해결하려면?
데이터바인딩은 해당 템플릿과 같은 vue의 js에서만 가져올 수 있음

단순히 data를 사본화하면 수정이 발생할때 전체수정을 해야함
따라서 data는 app.vue 등 상위 컴포넌트에 다 박아두고 하위 컴포넌트에서 (전송)가져다 씀(props)

props 3스탭
데이터 전송 / 등록 / 사용
1. 전송 : 상위컴포넌트 해당 호출 태그에 v-bind: 데이터(하위에서 쓸 변수) = "데이터"(그냥 : 도 됨)
                                                                        = "가져다 쓸 data key"
2. 등록 : 하위 export 안에 props:{데이터이름:자료형이름}으로 등록(String,Number,Object,Array)

근데 props는 read-only임
위에서 @onclick=""로 props를 재할당, 수정하면 안 됨

하위 컴포넌트에서 data를 만들어도 되지만
만약 상위 컴포넌트에서도 쓰는 data라면 상위컴포넌트에 만드는 것이 좋다
data는 하위로 전송하는 경우 props 쓰면 되지만 상위로 가는건 힘들다
-->