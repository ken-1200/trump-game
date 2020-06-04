<template>
  <div id="app">
    <Home></Home>
    <div class="btn">
      <button @click="shuffle">シャッフル</button>
    </div>
    <div class="container">
        <transition-group name="shuffle" tag="div" class="inner-box">
          <div class="box-items" v-for="(trump, index) in trumps" :key="index">
            <img :src="trump.isOpen ? trump.trumpInfo.frontRed.front : trump.trumpInfo.frontRed.back">
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
      trumps: []
    }
  },
  methods: {
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
    for(let i = 0; i < 53; i++) {
      console.log(i);
      if(0 < i && i < 14) {//1-13
        trump.trumpInfo.frontRed.front = require(`../src/assets/images/trump/${i}.gif`);
        trump.trumpInfo.frontRed.back = require(`../src/assets/images/trump/z01.gif`);
        console.log('black');
      }
      if(13 < i && i < 40) {//14-39
        trump.trumpInfo.frontRed.front = require(`../src/assets/images/trump/${i}.gif`);
        trump.trumpInfo.frontRed.back = require(`../src/assets/images/trump/z02.gif`);
        console.log('red');
      }
      if(39 < i && i < 53) {//40-52
        trump.trumpInfo.frontRed.front = require(`../src/assets/images/trump/${i}.gif`);
        trump.trumpInfo.frontRed.back = require(`../src/assets/images/trump/z01.gif`);
        console.log('black');
      }
      let trump = {
        isOpen: false,
        trumpInfo: {
          frontRed: 
          { 
            front: '',
            back: ''
          }
        }
      };
      this.trumps.push(trump);
    }
    this.trumps.push(require(`../src/assets/images/trump/x01.gif`));
    this.trumps.push(require(`../src/assets/images/trump/x02.gif`));
    this.trumps.push(require(`../src/assets/images/trump/z01.gif`));
    this.trumps.push(require(`../src/assets/images/trump/z02.gif`));
  },
  components: {
    Home
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
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  list-style-type: none;
  background-color: rgb(252, 244, 235);
}
.box-items {
  margin: 0 10px;
  padding: 10px 0;
}

img {
  width: 100%;
  border-radius: 15px;
  box-shadow: 0px 15px 12px 0px #2c3e50;
}
</style>
