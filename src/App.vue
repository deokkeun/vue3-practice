<!-- SFC(Single File Component) -->
<!-- 반응성(Reactivity) - 데이터를 갱신하면 화면도 바뀐다 -->
<template>
  <button @click="add1">
    add1
  </button>
  <h1 @click="increase">
    {{ count }} 클릭해보세요
  </h1>
  <!-- v- 디렉티브(Directive) -->
  <div v-if="count > 4">
    4보다 큽니다!(v-if 디렉티브, @click="")
  </div>
  <ul v-if="hasFruit">
    <h1>Fruits</h1>
    <!-- 데이터를 반복 할때는 각각의 데이터가 고유한지 나타내는 :key=""라는 속성을 작성해야 한다 -->
    <Fruit
      v-for="fruit in fruits"
      :key="fruit"
      :name="fruit">
      {{ fruit }}
    </Fruit>
  </ul>
  <section>
    <h1>Reverse Fruits</h1>
    <ul>
      <li
        v-for="fruit in reverseFruits"
        :key="fruit">
        {{ fruit }}
      </li>
    </ul>
  </section>
  <section>
    <h1 @click="changeMessage">
      {{ reversedMessage }}
    </h1>
    <h1>{{ reversedMessage }}</h1>
    <h1
      @click="changeStyle"
      :style="[fontStyle, backgroundStyle]">
      {{ msg }}
    </h1>
    <h1>{{ reversedMessage }}</h1>
  </section>
  <h1 
    v-html="msg"
    v-once
    @click="add">
  </h1>
  <h1 
    :[attr]="'active'"
    @[event]="add">
    {{ active }}
  </h1>
  <!-- isActive가 true 이면 active 클래스를 추가하고,
      isActive가 false 이면 active 클래스가 추가되지 않는다. -->
  <h1 
    :class="{ active: isActive }"
    @click="activate">
    Hello?!({{ isActive }})
  </h1>
</template>

<script>
import Fruit from '~/components/Fruit.vue'
export default {
  components: {
    // 객체 데이터의 내부에 속성의 이름과 데이터의 이름이 같으면 콜론 뒤는 생략 가능
    // Fruit: Fruit
    Fruit
  },
  data() {
    return {
      // Getter, Setter
      count: 0,
      // fruits: '',
      fruits: ['Apple', 'banana', 'Cherry'],
      msg: '<div style="color: red;">v-once, v-html 디렉티브</div>',
      active: ':[attr]="active"',
      attr: 'class',
      event: 'click',
      isActive: false,
      fontStyle: {
        color: 'orange',
        fontSize: '30px'
      },
      backgroundStyle: {
        backgroundColor: 'black'
      }
    }
  },
  // 계산된 데이터(computed)는 
  // 데이터 옵션(data())에 정의해 놓은 특정 데이터는 
  // 추가적으로 연산을 통해 정의한 데이터를 반환해서 
  // ex) return this.fruits.length > 0,  return fruit.split('').reverse().join('')
  // 사용할 수 있는 새로운 데이터 hasFruit(), reverseFruits()
  computed: { // 장점 : 캐싱(데이터 최적화(계산된 데이터)), !!!!!!읽기전용으로만 사용 가능!!!!!!!
    // Getter
    hasFruit() {
      return this.fruits.length > 0
    },
    reverseFruits() {
      return this.fruits.map(fruit => {
        return fruit.split('').reverse().join('')
      })
    },
    // 계산된 데이터는 캐싱 이라는 기능이 있기 때문에, 한번 연산해 놓은 값이 있으면
    // 반복적으로 화면에 출력할때, 다시 한번 연산하지 않는다
    // reversedMessage() { // Getter
    //   return this.msg.split('').reverse().join('')
    // }
    reversedMessage: { //Getter, Setter
      get() {
        return this.msg.split('').reverse().join('')
      },
      set(value) {
        this.msg = value
      }
    }
  },
  // 컴포넌트가 생성되기 직전에 실행되는 라이프사이클
  beforeCreate() {
    console.log('Before Create!', this.count); // Before Create! undefined
  }, 
  // 컴포넌트가 생성된 직후에 실행되는 라이프사이클(HTML구조에 연결되기 전)
  created() {
    console.log('Created!', this.count); // Created! 0
    console.log(document.querySelector('h1')); // null
  },
    // 컴포넌트가 HTML구조에 연결되기 전
  beforeMount() {
    console.log('Before Mount!'); // Before Mount!
    console.log(document.querySelector('h1')); // null
  },
  // 컴포넌트가 HTML구조에 연결된 후
  mounted() {
    console.log('Mounted!'); // Mounted!
    console.log(document.querySelector('h1')); // <h1>0</h1>
  },
  watch: { // 특정한 데이터들의 변경사항을 감시하는 옵션
    msg(value) {
      console.log('msg:', value)
    },
    // msg() {
    //   console.log('msg:', this.msg)
    // },
    count(value) {
      console.log("count:", value)
    },
    reversedMessage(value) {
      console.log("reversedMessage:", value)
    }
  },
  methods: {
    increase() {
      this.count += 1;
    },
    add() {
      this.msg += '!?'
      this.active += '!'
    },
    add1() {
      this.reversedMessage += '!?'
    },
    changeMessage() {
      this.msg = 'Good!'
    },
    activate() {
      this.isActive = true;
    },
    changeStyle() {
      this.fontStyle.color = 'red',
      this.fontStyle.fontSize = '50px'
    }
  }
}
</script>

<style scoped lang="scss">
  h1 {
    font-size: 50px;
    color: royalblue;
  }
  ul {
    li {
      font-size: 40px;
    }
  }
  .active {
    color: red;
    font-size: 100px;
    font-weight: bold;
  }
</style>