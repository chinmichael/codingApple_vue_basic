 <template>
  <div class="discount">
    <h4>지금 결제하면 {{ discountPer }}% 할인</h4>
  </div>
</template>

 <script>
export default {
  name: "Discount",
  props: {
    discountPer: Number,
  },
};
</script>
 
 <style>
div {
  box-sizing: border-box;
}

.discount {
  background-color: #eee;
  padding: 10px;
  margin: 10px;
  border-radius: 5px;
}
</style>

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

너무 많이 컴포넌트를 만들면 코드가 필연적으로 구조가 복잡해짐>>데이터 문제가 발생 쉬움
예를 들어 모달을 컴포넌트한다고 하면
modal이 상위 컴포넌트에서 가져온 데이터바인딩에 이래저래 문제가 터지기 쉬움

따라서 반복되는 UI에 사용하는것이 좋음

-->

<!--4/28 : Vue 컴포넌트의 Lifecycle : Discount컴포넌트 등장 후 알아서 사라지게 하기

Lifecycle을 왜? 어디에? 써야하나

<컴포넌트>는 웹페이지에 표시되기까지 일련의 과정을 거친다 = lifecycle
1.Create Step 2.Mount(2.Mount Step > Component 생성표시)
1.데이터만 존재하는 단계
2.template를 실제HTML로 바꿈(솔까 Vue template가 순수 HTML은 아니지) > index.html에 붙임

3.Update Step : data변경시 실시간 HTML재랜더링 >> 근데 정확히는 HTML이 아니라 컴포넌트가 재랜더링
4.Unmount Step : 컴포넌트가 삭제가 될때 (다른 페이지 이동 등)

Lifecycle을 대강이라도 알아야하는 이유는 저 step 사이에 hook을 걸어야하기 때문
각 단계 전에 실행해야 할 것을 hook을 걸어 세팅

beforeCreate()
created()
beforeMount()
mounted()
beforeUpdate()
updated()
beforeUnmount()
unmounted()

서버로 데이터를 요청할 때(Ajax 등으로) 이럴때도
lifecycle hook에 코드를 넣는다
보통 created()나(html 생성 전 데이터만 있을 때) mounted() 타이밍에 hook을 건다

-->