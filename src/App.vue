<template>
  <div id="app">
    <Home></Home>
    <div class="btn">
      <button @click="shuffle">シャッフル</button>
    </div>
    <div class="container">
        <transition-group name="shuffle" tag="div" class="inner-box">
          <div class="box-items" v-for="(trump, index) in trumps" :key="index" @click="open(trump, index)">
            <img :src="trump.isOpen ? trump.trumpInfo.front : trump.trumpInfo.back">
          </div>
      </transition-group>
    </div>
  </div>
</template>

<script>
import Home from './components/Home.vue'

export default {
  data() {
    return {
      trumps: [],
      openCounter: 0
    }
  },
  methods: {
    open(trump, index) {
      if(this.openCounter + 1 > 2) return;//2枚以上押せないようにする
      trump.isOpen ? trump.isOpen = false : trump.isOpen = true;//めくる処理
      this.openCounter++;
      if(this.openCounter === 2) {//resetメソッドを呼ぶ
        this.reset(trump);
      }
      console.log(index);
    },
    shuffle() {
      var leng = this.trumps.length;//lengthをとる
      while(leng > 0) {
        var rand = Math.floor(Math.random() * leng);//random数
        var temp = this.trumps[leng - 1];//初期値
        this.trumps[leng - 1] = this.trumps[rand];//random数を代入
        this.trumps[rand] = temp;//random数を調整ex.)1-55まで
        leng -= 1;//0になるまで-1
      }
      this.trumps.push();
    }
  },
  mounted() {
    for(let i = 0; i < 55; i++) {
      console.log(i);
      if(0 < i && i < 14) {//1-13
        trump.trumpInfo.front = require(`../src/assets/images/trump/${i}.gif`);
        trump.trumpInfo.back = require(`../src/assets/images/trump/z01.gif`);
        console.log('black');
      }
      if(13 < i && i < 40) {//14-39
        trump.trumpInfo.front = require(`../src/assets/images/trump/${i}.gif`);
        trump.trumpInfo.back = require(`../src/assets/images/trump/z02.gif`);
        console.log('red');
      }
      if(39 < i && i < 53) {//40-52
        trump.trumpInfo.front = require(`../src/assets/images/trump/${i}.gif`);
        trump.trumpInfo.back = require(`../src/assets/images/trump/z01.gif`);
        console.log('black');
      }
      if(53 === i) {//jokerBlack
        trump.trumpInfo.front = require(`../src/assets/images/trump/x02.gif`);
        trump.trumpInfo.back = require(`../src/assets/images/trump/z01.gif`);
        console.log('blackJoker');
      }
      if(54 === i) {//jokerRed
        trump.trumpInfo.front = require(`../src/assets/images/trump/x01.gif`);
        trump.trumpInfo.back = require(`../src/assets/images/trump/z02.gif`);
        console.log('redJoker');
      }
      let trump = {
        isOpen: true,
        trumpInfo: {
          front: '',
          back: ''
        }
      };
      this.trumps.push(trump);
    }
  },
  components: {
    Home
  }
}
</script>

<style scoped>
/* fade-move */
.fade-move {
  transition: transform 1s;
}
/* shuffle-transition */
.shuffle-enter, 
.shuffle-leave-to {
  /* 現れる時の最初の状態, 消えるときの最後の状態 */
  opacity: 0;
}
.shuffle-enter-active,
.shuffle-leave-active {
  /* 現れる時のトランジションの状態, 消えるときのトランジションの状態 */
  transition: opacity 3s;
}
.shuffle-enter-to,
.shuffle-leave {
  /* 現れる時の最後の状態, 消える時の最初の状態 */
  opacity: 1;
}

@keyframes sk-rotatetrumps {
  0% {
    transform: rotateY(0deg);
  }
  50% {
    transform: rotateY(0deg);
  }
  100% {
    transform: rotateY(-180deg);
  }
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 100%;
  max-width: 80%;
  margin: auto;
}

.btn {
  background-color: rgb(231, 214, 193);
}

button {
  font-weight: 600;
  padding: 10px 40px;
  margin: 30px auto;
  color: #2c3e50;
  cursor: pointer;  
  background-color: white;
  border: 1px solid #2c3e50;
  transition: all 0.26s;
}

button:hover {
  transform: translate(-2.5px, -2.5px);
  box-shadow: 5px 5px 10px 0 #2c3e50;
}

.container {
  background-color: rgb(252, 244, 235);
}

.inner-box {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.box-items {
  margin: 0 10px;
  padding: 10px 0;
  /* animation: sk-rotatetrumps 3s ease-in-out forwards; */
  /* animation-delay: 0.74s; */
}

img {
  width: 100%;
  border-radius: 15px;
  box-shadow: 0px 15px 12px 0px #2c3e50;
}
</style>
