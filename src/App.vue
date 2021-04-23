<template>
  <!-- <div
    class="black-bg"
    v-if="modal_status == true"
    v-on:click="modal_status = false"
  >
    <div class="white-bg">
      <img :src="oneroom[modal_index].image" class="room-modal-img" />
      <h4>{{ oneroom[modal_index].title }}</h4>
      <p>{{ oneroom[modal_index].price }}원</p>
      <p>{{ oneroom[modal_index].content }}</p>
      <button v-on:click="moadl_status = false">close</button>
    </div>
  </div> -->

  <Modal
    v-bind:oneroom="oneroom"
    v-bind:modal_index="modal_index"
    :modal_status="modal_status"
  />

  <div class="menu">
    <a v-for="menu_name in menu_item" :key="menu_name">{{ menu_name }}</a>
    <!--<a v-for="(a, i) in menu_item" :key="i">{{a}}</a>-->
  </div>

  <Discount></Discount>

  <div>원룸샵</div>
  <Card
    v-for="(a, i) in oneroom"
    :key="i"
    v-bind:oneroom_list="oneroom[i]"
  ></Card>
  <!--
  <div class="product-list" v-for="(a,i) in products" :key="i">
    <img src="./assets/room0.jpg" class="room-img">
    <h4 :style="style1" @click="modal_status = true">{{a}}</h4>
    <p>{{prices[i]}} 만원</p>
    <button v-on:click="declare_cnt[i]++">허위매물신고</button> <span>신고수 : {{declare_cnt[i]}}</span>
  </div>-->

  <div>원룸샵</div>
  <!-- <div class="product-list" v-for="(a, i) in oneroom" :key="i">
    <img :src="oneroom[i].image" class="room-img" />
    <h4
      @click="
        modal_status = true;
        modal_index = i;
      "
      :style="style1"
    >
      {{ oneroom[i].title }}
    </h4>
    <p>{{ oneroom[i].price }}원</p>
  </div> -->

  <!--a로 세팅하는 경우
    <div class="product-list" v-for="(a, i) in oneroom" :key="i">
    <img :src="a.image" class="room-img">
    <h4 :style="style1" @click="modal_status = true">{{a.title}}</h4>
    <p>{{a.price}}원</p>
  </div> -->

  <!--<div>
    <h4 :style="style1">{{product[1]}}</h4>
    <p>{{price2}} 만원</p>
  </div>

    <div>
    <h4 :style="style1">{{product[2]}}</h4>
    <p>{{price2}} 만원</p>
  </div>-->
</template>

<script>
import { roomInform } from "./assets/oneroom.js";
import Discount from "./Discount.vue";
import Modal from "./Modal.vue";
import Card from "./Card.vue";

export default {
  name: "App",
  data() {
    // 데이터 바인딩을 위해 따로 변수하는게 아닌 이런 data보관소를 만들어야함 (Object 자료형)
    return {
      modal_status: false, //리액트에서는 state라고 함
      modal_index: 0, //이것도 state

      menu_item: ["Home", "Shop", "About"],
      //prices : [70, 80, 90],
      //products : ['역삼동 원룸', '천호동 원룸', '마포구 원룸'],
      oneroom: roomInform, // Array안에 Object로 구성되었다고 생각[{},{}]
      style1: "color : blue", //속성 바인딩도 물론 문자로,
      declare_cnt: [0, 0, 0],
    };
  },
  method: {
    increase_declare(i) {
      // 이벤트 호출시는 ()안붙일 수 있음
      this.declare_cnt[i]++; //Vue 함수 주의사항 : data사용시 반드시 this. 붙임
    },
  },
  components: {
    Discount: Discount, //key와 value가 같다면 key만 적어도 됨(ES6 문법)
    Modal,
    Card: Card,
  },
};
</script>

<style>
body {
  margin: 0;
}

div {
  box-sizing: border-box;
}

/* .black-bg {
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
} */

.room-img {
  width: 100%;
  margin-top: 40px;
}

/* .room-modal-img {
  width: 300px;
  margin-top: 40px;
} */

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.menu {
  background-color: darkslateblue;
  padding: 15px;
  border-radius: 5px;
}

.menu a {
  color: white;
  padding: 10px;
}
</style>

<!--메인 페이지 template(HTML) / script(JS) / style(CSS)로 구성됨 : 실제 틀어지는건 index.html로 가지만-->

<!-- 실시간 미리보기 : 터미널 npm run serve-->

<!--일단 기본적으로 html, css, js은 기본대로 가면 됨 / 기능개발만 뷰 문법 적용-->

<!-- 4/20
오늘 주요 문법 주제1 : 데이터 바인딩
JS 데이터를 HTML에 넣기 위한 문법
기존 document.getElementById().innerHTML = ?? 이를 간편하게 함
>>
JS에 data(){}를 만들어 Object형으로 데이터를 넣고 HTML에 {{데이터 key}}로 바인딩함

Tip : 코딩공부는 문법 이론이 아니라 왜 이 문법을 쓰는지를 공부해야 함
>>데이터 바인딩의 경우 : HTML하드코딩(아예 거기다 박으면) 나중에 변경이 어려움
                       Vue가 제공하는 실시간 자동 렌더링 기능을 쓰기 위함(존나중요)
                       >> Vue는 data를 변경하면 HTML에 실시간으로 자동 랜더링 >> 웹 앱의 중요 요소
                       >> 예를 들어 위의 경우 값 변경 버튼이 있다면 클릭으로 JS데이터 수정>실시간 랜더링

                       물론 그닥 변치 않은 데이터는 그냥 하드코딩한다(노쓸모라)

참고로 HTML의 속성(property)도 데이터바인딩이 가능함
vue의 경우는 속성타입 앞에 ':'를 붙이고 키를 불러냄 (angular는 []였지....)
-->

<!-- 4/20
오늘 주요 문법 주제2 : vue 반복문
<태그 v-for="작명 in 횟수" :key="작명">

<태그 v-for="작명 in data" :key="작명"> : 간단히 배열 반복문 가능
                                         forEach랑 비슷하게 생각 '작명'이 변수명이라 생각
<tag v-for="i" in data :key="i">{{i}}</tag>

:key="" 반복문 생성 시 반복문 요소를 컴퓨터가 구분하기 위함(변수여야 함)
작명은 (left,right)를 통해 2개까지 가능
이 경우 left는 array내의 데이터, right는 1씩 증가하는 정수 
-->

<!--4/21 주요문법 : 이벤트 리스너, 핸들러 click감지
어떤 액션을 취하면 특정 기능을 실행시키는것

보통 HTML클릭시 코드실행시
기존 JS : onClick="";
Vue : v-on:click=""; or @click=""; @ = v-on:

위 신고버튼 작성한다면
기존 JS : 버튼을 누르면 해당 수를 찾아 +1 >> 그리고 그것을 HTML요소에 다시반영
Vue : 해당 data만 수정되게 만들면 됨(실시간 랜더링 위에 다시반영 필요없음)
    : v-on:click="data_key++";(or data_key += 1) 만 하면됨 

당연히 함수도 됨(긴 코드를 함수명으로 축약시켜 HTML에 표현하기 위해)
>>data 다음 공간에 method:{ 함수명(){} }를 만든다

그밖에 여러 이벤트
@(v-on:)
mouseover : 커서를 갖다될때
drag, input 등

-->

<!--4/21 주요문법 : vue 조건문 / 모달창
Vue 개발 팁 : 항상 데이터중심적으로 (데이터를 어떻게 만들지를 먼저 생각)
img넣는 경로는 assets에 넣는다

router를 설치할 경우 굳이 모달창이 아니라도 다른 페이지로 이동시키면 됨

모달창 제작 스탭(동적 UI 제작 스탭)
0.HTML CSS 디자인
1.UI의 현재상태를 데이터로 저장해둠 (모달창이 현재 보이는지 등)
>>data에 modal_status 작성(true, false 등)
2.데이터에 따라 UI가 어떻게 보일지 작성
>>해당 HTML 요소에 v-if(vue 조건문) = ""로 조건 따른 변화 처리

-->

<!-- 4/22 주요문법1 : import, export

실제 서버에서 데이터를 가져와서 HTML에 꽂는법
근데 지금은 서버세팅이 안되어있기에 강좌에서 데이터 받아놓음
>> 파일을 따로보관하여 가져옴 >> import문, export문

자바스크립트 파일끼리 데이터 변수 전달할 때 (물론 함수도 전달 가능)
보내는 곳 : export
var apple = 10;을 보낼 때
var apple2 = 10;
export default(보낼것이 하나일 때) apple / export {apple, apple2} 

받는 곳 : import
import 작명(default로 보내면 짜피 한개이므로) from "경로"; >> 갖고온 변수를 써야함 (안그럼 에러)
import {apple} from "경로"; import {apple, apple2} from "경로";

바인딩 다시 정리
HTML 태그 안 내용 바인딩 : {{}} // 이건 Angular도 같음
HTML 태그 속성에 바인딩 : ':' (:image) // angular의 경우는 []
HTML 태그 이벤트 : v-on:이벤트 (이벤트도 일종의 속성이니 :) or @이벤트 // 엥귤러의 경우 () 

-->

<!-- 4/22 주요문법2: 모달창에 데이터 넣기
v-for 반복문에 따라 Array의 index가 바뀌는걸 모달창에 적용하는 경우

data에 이를 위한 index status을 마련한다(state) : 일종의 이것도 동적 UI

1.UI의 현재상태를 데이터로 저장해둠 (모달창이 현재 보이는지 등)
>>data에 modal_status 작성(true, false 등)
2.데이터에 따라 UI가 어떻게 보일지 작성
>>해당 HTML 요소에 v-if(vue 조건문) = ""로 조건 따른 변화 처리


@click 등 이벤트에 여러 함수 등 적용시 style등 프로퍼티에 여러개 적용시키는거처럼 ';'이용

v-if가 있다면 v-else-if v-else도 존재
<div v-if = ""></div>
<div v-else-if = ""></div>
<div v-else></div>

사용 방법은 저렇게 하면 알아서 HTML에 직접 if문 세팅된다고 생각

-->

<!-- 4/23 : 컴포넌트

컴포넌트 쓰임 : HTML을 줄이기 위함 (한 단어로 축약)
새롭게 .vue파일을 생성 후 template, script(name도 지정), style 구조를 세팅한 뒤에
필요한 컴포넌트 구성 후 불러온다
(angular는 3파일로 기본 나누지만 vue는 .vue에 다 박음)

축약한 컴포넌트 사용방법
1.vue 파일 import(당빠 가져다 쓰는 곳 script)
2.등록 (스크립트의 컴포넌트 파트)
3.사용

컴포넌트 사용 이유 : 1.코드정리 2.재사용


-->