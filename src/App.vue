<template>
  <div id="app">
    <Home></Home>
    <div class="btn">
      <button @click="shuffle">シャッフル</button>
    </div>
    <div class="result">
      <span class="player">あなたが取得したトランプ{{ getPlayers }}組</span>
      <span class="computer">相手が取得したトランプ{{ getComputers }}組</span>
    </div>
    <div class="container">
      <div class="get-trumps">
        <div class="inner-boxs">
          <transition-group name="shuffle" tag="div" class="inner-items">
            <div class="box-items" v-for="(match, index) in isMatchTrump" :key="index">
              <img class="img-match" :src="match.trumpInfo.front">
            </div>
          </transition-group>
        </div>
        <div class="inner-computer">
          <transition-group name="shuffle" tag="div" class="inner-items">
            <div class="box-items" v-for="(match, index) in isMatchComputer" :key="index">
              <img class="img-match" :src="match.trumpInfo.front">
            </div>
          </transition-group>
        </div>
      </div>
      <transition-group name="shuffle" tag="div" class="inner-box">
        <div class="box-items" v-for="(trump, index) in trumps" :key="index" @click="open(trump)" v-show="trump.isGet == null">
          <img :src="(trump.isOpen || trump.isGet != null) ? trump.trumpInfo.front : trump.trumpInfo.back">
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
      isMatchTrump: [],
      isMatchComputer: [],
      isNotMatchTrump: [],
      openCounter: 0,
      player: "player",
      computer: "computer"
    }
  },
  computed: {
    getPlayers() {
      if(!this.trumps || this.trumps.length === 0) return;//全ての組みを取り終わった時
      return (
        this.trumps.filter(trump => {//配列をフィルタリング(引数に与えられた関数を実行して、この条件と一致している全ての配列要素からなる新しい配列を生成する)
          return trump.isGet === this.player;
        }).length / 2
      );
    },
    getComputers() {
      if(!this.trumps || this.trumps.length === 0) return;
      return (
        this.trumps.filter(trump => {
          return trump.isGet === this.computer;
        }).length / 2
      );
    }
  },
  methods: {
    isComputer() {
      let comOpenTrumps = [];
      this.trumps.forEach((trump) => {
        if(trump.isGet == null) {
          comOpenTrumps.push(trump);
        }
      });
      let firstRanId = Math.floor(Math.random() * comOpenTrumps.length);//その長さを54ランダム
      let secondRanId = Math.floor(Math.random() * comOpenTrumps.length);
      if(firstRanId == secondRanId) {//もし同じランダムになったらもう一度
        firstRanId = Math.floor(Math.random() * comOpenTrumps.length);//その長さを54ランダム
        secondRanId = Math.floor(Math.random() * comOpenTrumps.length);
        if(firstRanId == secondRanId) {
          alert('リロードしてください！');
        }
      }
      let firstComId = comOpenTrumps[firstRanId];
      let secondComId = comOpenTrumps[secondRanId];
      let computerFirst = [];
      let computerSecond = [];
      comOpenTrumps.forEach((comTrumps) => {
        setTimeout(() => {
          if(firstComId.trumpInfo.id === comTrumps.trumpInfo.id) { 
            comTrumps.isOpen = true; 
          }
          if(secondComId.trumpInfo.id === comTrumps.trumpInfo.id) { 
            comTrumps.isOpen = true;  
          }
          let abs = ((firstComId.trumpInfo.id + 13) - (secondComId.trumpInfo.id + 13));
          if(abs % 13 === 0) {
            if(firstComId.trumpInfo.id === comTrumps.trumpInfo.id) {//randomの数値と同じidのトランプの時に表 1枚目
              comTrumps.isGet = this.computer;
              computerFirst.push(comTrumps);
              computerFirst.forEach((trumps) => {
                this.isMatchComputer.push(trumps);
              });
            }
            if(secondComId.trumpInfo.id === comTrumps.trumpInfo.id) {//2枚目
              comTrumps.isGet = this.computer;
              computerSecond.push(comTrumps);
              computerSecond.forEach((trumps) => {
                this.isMatchComputer.push(trumps);
              });
            }
          }else {
            if(abs % 13 !== 0) {//マッチしていない時
              this.isNotMatchTrump.push(firstComId);
              this.isNotMatchTrump.push(secondComId);
            }
            this.reset();
          }
        }, 2300);
      });
    },
    isMatch() {
      let openTrumps = [];
      this.trumps.forEach((trump) => {//forEachで各配列の要素
        if(trump.isOpen && trump.isGet == null) {//true && null / 表 && まだ誰も取っていない
          openTrumps.push(trump);
        }
      });
      let firstId = openTrumps[0];//１枚目
      let secondId = openTrumps[1];//２枚目
      let abs = ((firstId.trumpInfo.id + 13) - (secondId.trumpInfo.id + 13));//絶対値の判定
      if(abs % 13 === 0) {//13の倍数
        firstId.isGet = this.player;
        secondId.isGet = this.player;
        openTrumps.forEach((matchTrump) => {
          this.isMatchTrump.push(matchTrump);
        });
      }
      if(firstId.trumpInfo.id === 'jokerBlack' && secondId.trumpInfo.id === 'jokerRed') {
        firstId.isGet = this.player;
        secondId.isGet = this.player;
        openTrumps.forEach((matchTrump) => {
          this.isMatchTrump.push(matchTrump);
        });
      }else {
        if(abs % 13 !== 0) {//マッチしていない時
          openTrumps.forEach((noMacthTrumps) => {
            this.isNotMatchTrump.push(noMacthTrumps);
          });
          this.isComputer();//相手
        }
      }
      this.reset();
    },
    reset() {
      setTimeout(() => {
        this.isNotMatchTrump.forEach((trump) => {
          if(trump.isOpen && trump.isGet == null) {
            trump.isOpen = false;
          }
        });
        this.openCounter = 0;//初期化
      }, 1200);
    },
    open(trump) {
      if(this.openCounter + 1 > 2) return;//2枚以上押せないようにする
      trump.isOpen ? trump.isOpen = false : trump.isOpen = true;//めくる処理
      this.openCounter++;
      if(this.openCounter == 2 && trump.isOpen == false) {//同じトランプを２回クリックした時
        this.openCounter = 0;
      }else if(this.openCounter == 2) {//2回目
        this.isMatch(trump);
      }
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
    for(let i = 1; i < 55; i++) {
      let trump = {
        isOpen: false,
        isGet: null,
        trumpInfo: {
          front: '',
          back: '',
          id: ''
        }
      }
      if(0 < i && i < 14) {//1-13
        trump.trumpInfo.front = require(`../src/assets/images/trump/${i}.gif`);
        trump.trumpInfo.back = require(`../src/assets/images/trump/z01.gif`);
        trump.trumpInfo.id = i;
      }
      if(13 < i && i < 40) {//14-39
        trump.trumpInfo.front = require(`../src/assets/images/trump/${i}.gif`);
        trump.trumpInfo.back = require(`../src/assets/images/trump/z02.gif`);
        trump.trumpInfo.id = i;
      }
      if(39 < i && i < 53) {//40-52
        trump.trumpInfo.front = require(`../src/assets/images/trump/${i}.gif`);
        trump.trumpInfo.back = require(`../src/assets/images/trump/z01.gif`);
        trump.trumpInfo.id = i;
      }
      if(53 === i) {//jokerBlack
        trump.trumpInfo.front = require(`../src/assets/images/trump/x02.gif`);
        trump.trumpInfo.back = require(`../src/assets/images/trump/z01.gif`);
        trump.trumpInfo.id = 'jokerBlack';
      }
      if(54 === i) {//jokerRed
        trump.trumpInfo.front = require(`../src/assets/images/trump/x01.gif`);
        trump.trumpInfo.back = require(`../src/assets/images/trump/z02.gif`);
        trump.trumpInfo.id = 'jokerRed';
      }
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
  transition: opacity 1s;
}
.shuffle-enter-to,
.shuffle-leave {
  /* 現れる時の最後の状態, 消える時の最初の状態 */
  opacity: 1;
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

.result {
  position: relative;
}

.get-trumps {
  position: relative;
}

.inner-computer {
  width: 50%;
  height: 250px;
  position: absolute;
  right: 0;
  top: 0;
}

.player {
  display: flex;
  justify-content: center;
  width: 50%;
}

.computer {
  position: absolute;
  right: 0;
  top: 0;
  left: 50%;
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
}

img {
  height: 114px;
  object-fit: contain;
  border-radius: 6px;
  box-shadow: 0px 15px 12px 0px #2c3e50;
}

.inner-boxs {
  width: 50%;
  height: 250px;
}

.inner-items {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  width: 50%;
  height: 250px;
}

.img-match {
  position: absolute;
  height: 100px;
  object-fit: contain;
  border-radius: 4px;
  box-shadow: 0px 6px 12px 0px #a7a8aa;
}
</style>
