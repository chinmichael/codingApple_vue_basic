 <template>
  <div class="discount">
    <h4>지금 결제하면 20% 할인</h4>
  </div>
</template>

 <script>
export default {
  name: "Discount",
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