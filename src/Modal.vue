<template>
  <div class="black-bg" v-if="modal_status == true">
    <!-- v-on:click="modal_status = false" 
  > -->
    <div class="white-bg">
      <img :src="oneroom[modal_index].image" class="room-modal-img" />
      <h4>{{ oneroom[modal_index].title }}</h4>
      <p>{{ oneroom[modal_index].content }}</p>
      <!-- <input @input="month = $event.target.value" /> -->
      <!--물론 이걸 바로 console.log로 볼수는 없으니 밑에 function을 통해서 볼 수 있음-->
      <input v-model.number="month" id="month_input" />
      <!--숫자로 변환되 저장되나 문자를 막아주는건 아님-->
      <p>{{ month }}개월 선택 : {{ oneroom[modal_index].price * month }}원</p>
      <!-- <button v-on:click="modal_status = false">close</button> -->
      <button
        @click="
          $emit('closeModal');
          month = 1;
        "
      >
        close
      </button>
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
      month: 1, // 초기값이 number라 문자로 넘겨받긴해도 최종적으로 number에 해당되야 정상인식
    };
  },
  watch: {
    month(afterData, beforeData) {
      if (isNaN(afterData)) {
        this.month = beforeData; //Vue, Angular 등에서
        alert("숫자만 입력 가능합니다");
      }
      if (afterData > 12 || afterData < 1) {
        this.month = beforeData;
        alert("12개월까지 입력이 가능합니다");
      }
    },
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

<!--4/24 : props 추가
<컴포넌트 :데이터변수명="데이터"/>에서

데이터는 data()에서 보내는 변수명 외에도
직접적으로 데이터 입력도 가능 데이터변수명=""
Array Object 등도 다양한 자료형 가능

만약 단순한 문자형 자료형을 보낼때는 바인딩 처리 안함 데이터변수명="문자열자료형"
만약 x="1234" 이면 문자열로 1234보내지고 :x="1234"로 하면 number로 보내짐

추가된 Object 내부요소 따로따로 보내는거 축약
ex) human = {name:'kim', age:20} 일때
    기존에 따로따로 보내는 경우 <컴포넌트 :name="human.name" :age="human.age"/>
    <컴포넌트 v-bind="human"> 이걸로 간략화 되긴 함 (별로 쓸일은 없음)
-->

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


결국 상위컴포넌트 데이터를 수정하는수밖에 읍따
-->

<!--4/26 : 사용자 input 다루기 (@input, v-model)

  input시 이벤트 발생 시키려면
  v-on:click (= @click) 처럼 input에도 핸들러를 달아주면 됨(@input)
  <input> 태그 내 가능한 input 핸들러 @input(입력할 때마다) @onchange(입력이 끝나고 다른 행동을 취할 때)
  여기도 $evnet 사용

  $event는 아래 이벤트 리스너에서 >> e와 동일한 역할 즉 요소의 변수라고 생각
  HTMLelements.addEventListner.('event', function(e) {
    e.target(); // 이벤트를 일으킨 요소
    cf) e.preventDefault() // 이벤트를 일으키지 않은거처럼 막음
  });

  v-model은 축약버전
  위 @input="data=$event.taget.value" 즉 form입력값은 밑 data에 저장하기의 축약버전
  v-model="data"와 같음

  v-model은 여러 input 뿐 아니라 textarea,select 등에도 사용 가능
  input checkbox의 경우 true false로 받아옴

  data세팅에는 초기값이 중요
  위 month에서 문자 입력시 인식 못하는 이유(짜피 문자열로 넘겨받긴 하지만)
  최종적으로 number가 들어와야하는 것으로 인식한다.
  일단 JS특성상 자료형이 워낙 자유롭긴 하니 문자열자료형으로 저장되나 형식은 number라 생각
  따라서 위에서 price와 *가 아니라 +로 하면 문자열 합치기가 되버림

  이런 경우 >> v-model.number로 하면 숫자로 바꿔달라는 것으로 정의가능하긴 함 (문자입력을 막을순 없지만)

-->

<!-- 4/26 watcher 이용 : input에 입력값 유효성 검사

watcher : data감시 함수 (데이터 변화가 제대로 되는지, 알맞는 데이터가 들어오는지)

export{} 안에 watch : {}를 만들어서 세팅
watch:{} 안에 감시하고자 하는 데이터 변수명으로 함수명을 지어 검사를 시킨다
(위의 경우 month 데이터 감시하므로 month() {}) 해당 함수의 파라미터는 데이터에 넣는 입력값
watcher함수의 파라미터는 2개까지 가능 (변경 후 데이터, 변경 전 데이터)로 받는다

>> 해당 데이터가 변경이 될때마다 해당 watcher함수가 실행된다

cf) isNaN() / Number.isNaN() >> 전자는 파라미터를 Number형을 강제형전환후 판단한다
    input으로 들어오는 데이터는 일단 문자열

cf) <input type="range" min="1" max="12"> : 1~12를 막대로 선택 가능한 range 형성

만약 watcher를 사용하기 귀찮은 경우(주로 많이쓰는거 정규식 설정땜에)
Vue 전용 form validation 라이브러리 ㄱㄱ
-->