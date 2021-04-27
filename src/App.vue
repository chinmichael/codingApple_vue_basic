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

  <!-- <div class="start" :class="{ end: modal_status }"> -->
  <transition name="fade">
    <Modal
      v-bind:oneroom="oneroom"
      v-bind:modal_index="modal_index"
      :modal_status="modal_status"
      @closeModal="modal_status = false"
    />
  </transition>
  <!-- </div> -->

  <!--4/27 vue로 애니메이션 주기

      css애니메이션
      1.시작전 class명 css작성
      2.애니메이션 끝난 뒤 class명 css작성
      3.원할때 2번 class명 부착 >> vue에서 클래스 바인딩(:class)을 통해

      클래스명을 조건부로 넣어주는 경우 위 클래스 바인딩에 {클래스명 : 조건} >. 조건이 true일 때 부착됨

      위 케이스를 Vue에서는 <transition>을 통해 더 쉽게 줄 수가 있다
      보통 등장 애니메이션(v-if)에 잘 줌
      <transition name="">태그로 필요한 부분 감싼뒤
      style에 위 name으로 3개의 class작성
      .name-enter-from(시작 애니메이션 동작전 상태)
      .name-enter-active(transition 애니메이션 동작중 상태 주로 transition같은거)
      .name-enter-to(끝 애니메이션 동작 후 상태)

      퇴장 애니메이션도 쉽게 세팅 가능
      위 클래스에서 enter가 아니라 leave로 작성

  -->

  <div class="menu">
    <a v-for="menu_name in menu_item" :key="menu_name">{{ menu_name }}</a>
    <!--<a v-for="(a, i) in menu_item" :key="i">{{a}}</a>-->
  </div>

  <Discount></Discount>

  <div>원룸샵</div>

  <button @click="abcSort">가나다순 정렬</button>
  <button @click="lowPriceSort">낮은가격순 정렬</button>
  <button @click="upPriceSort">높은가격순 정렬</button>
  <button @click="underPrice(50)">50만원 이하만 보여주기</button>
  <button @click="sortBack">되돌리기</button>

  <!-- 4/27 : Vue 정렬 + 원본 데이터 보존

  기존 JS : 1.Array 데이터를 정렬 2.HTML에 반영

  Vue : Array 데이터 정렬하고 끝 / 데이터바인딩이 되면 실시간 자동 렌더링이 되기 때문에

  -->

  <Card
    v-for="(a, i) in oneroom"
    :key="i"
    v-bind:oneroom_list="oneroom[i]"
    v-bind:index="i"
    @openModal="
      modal_status = true;
      modal_index = $event;
    "
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
      oneroom_original: roomInform, // [...Array] array/object 데이터 별개 사본 저장
      oneroom: [...roomInform], // Array안에 Object로 구성되었다고 생각[{},{}]
      style1: "color : blue", //속성 바인딩도 물론 문자로,
      declare_cnt: [0, 0, 0],
    };
  },
  methods: {
    // increase_declare(i) {
    //   // 이벤트 호출시는 ()안붙일 수 있음
    //   this.declare_cnt[i]++; //Vue 함수 주의사항 : data사용시 반드시 this. 붙임
    // },

    lowPriceSort() {
      this.oneroom.sort(function (a, b) {
        return a.price - b.price;
      });
    },

    // sort()문법 : 실은 문자정렬임
    // 만약 숫자 정렬의 경우 .sort(function(a,b){return a-b}); << 이 함수를 고대로 가져와 적용
    // sort()의 동작원리 : array의 데이터 비교 결과가 음수면 앞(a) 파라미터를 왼쪽으로 보냄
    // sort()은 원본데이터를 변형시켜버림 (map(), filter()는 원본보존) >> 좋지 않음

    upPriceSort() {
      this.oneroom.sort(function (a, b) {
        return b.price - a.price;
      });
    },

    abcSort() {
      this.oneroom.sort(function (a, b) {
        if (a.title > b.title) return 1;
        if (a.title < b.title) return -1;
        return 0;
      });
    },

    // 문자열 정렬의 경우 저런식으로 두 요소를 비교해 1,-1,0을 리턴하게 하는 방식으로 소팅함

    underPrice(standard) {
      this.oneroom = this.oneroom.filter(function (e) {
        return e.price <= 10000 * standard;
      });
    },

    // filter의 경우는 걸러진 결과 배열 = 대상배열.filter(function(e) {
    //  return 걸러지는 공식});으로 처리함 위를 기준으로 보자

    sortBack() {
      this.oneroom = [...this.oneroom_original];
      //spread operator 문법 array나 object에 ...붙이면 이 둘의 괄호[], {}을 제거해줌
      //그상태로 다시 괄호를 씌우면(그래서 위 배열은 [...]을 붙인 모양새) 같은 곳을 참조하지 않게 처리됨
      //즉 같은 내용의 새로운 주소를 참조시킬 수가 있음

      //문제는 저런식으로 하면 oneroom이 이제 oneroom_original을 참조하고 다시 sort하면 이것마저 바뀜
      //따라서 넣는 순간 사본을 만들어 참조시키는 방식으로 해야한다.
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

/* .start {
  opacity: 0;
  transition: all 1s;
}
.end {
  opacity: 1;
} */

.fade-enter-from {
  transform: translateY(-1000px); /*y축 세팅*/
  /* opacity: 0; */
}

.fade-enter-active {
  transition: all 1s;
}

.fade-enter-to {
  transform: translateY(0px); /*y축 세팅*/
  /* opacity: 1; */
}

.fade-leave-from {
  opacity: 1;
}

.fade-leave-active {
  transition: all 1s;
}

.fade-leave-to {
  opacity: 0;
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