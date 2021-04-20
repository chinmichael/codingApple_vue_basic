<template>
  <div class="menu">
    <a v-for="menu_name in menu_item" :key="menu_name">{{menu_name}}</a>
    <!--<a v-for="(a, i) in menu_item" :key="i">{{a}}</a>-->
  </div>

  <div>원룸샵</div>
  <div class="product-list" v-for="(a,i) in product" :key="i">
    <h4 :style="style1">{{a}}</h4>
    <p>{{price[i]}} 만원</p>
  </div>

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

export default {
  name: 'App',
  data() { // 데이터 바인딩을 위해 따로 변수하는게 아닌 이런 data보관소를 만들어야함 (Object 자료형)
    return {
      menu_item : ['Home', 'Shop', 'About'],
      price : [70, 80, 90],
      product : ['역삼동 원룸', '천호동 원룸', '마포구 원룸'],
      style1 : 'color : blue', //속성 바인딩도 물론 문자로
    }
  },
  components: {
  }
}
</script>

<style>
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