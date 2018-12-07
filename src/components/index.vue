<template>
  <div class="index">
    <div id="headbar">
      <img src="../assets/cc.jpg">
      <div class="data">
        <p>Participant<br><span>8人</span></p>
        <p>Cumulativevote<br><span>{{sum}}票</span></p>
        <p>Team<br><span>2队</span></p>
      </div>
    </div>
    <div id="mid">
        <p class="ques">{{ques}}</p>
        <div class="v">
          <p class="num1">{{num1}}</p>
          <p class="num2">{{num2}}</p>
        </div>
        <div class="voting">
          <div class="leftv"></div>
          <div class="rightv" v-bind:style="{width:num2w+'%'}"></div>
        </div>

        <div class="team">
          <p class="num1">{{slogan1}}</p>
          <p class="num2">{{slogan2}}</p>
        </div>

        <div class="time">
          <img src="../assets/compete-icon.png">
          <p>There's still time for the vote to end:<br>{{day}} Day {{h}} Hour {{minu}} Minute {{second}} Second</p>
        </div>
    </div>
    <div id="tail">
      <p>西二在线West2online</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "index",
  data() {
    return {
      num1: 0,
      num2: 0,
      ques: "",
      slogan1: "",
      slogan2: "",
      day:'',
      minu:'',
      second:'',
    };
  },
  created() {
    this.rev();
    this.gettime();
  },
  computed: {
    num2w: function() {
      let n1 = parseInt(this.num1);
      let n2 = parseInt(this.num2);
      return n2 / (n1 + n2) * 100;
    },
    sum:function(){
      let n1 = parseInt(this.num1);
      let n2 = parseInt(this.num2);
      return n1+n2;
    }
  },
  methods: {
    timedel:function(){
      var $this=this;
      if($this.second!=0){
        $this.second--;
      }
      else{
        if($this.minu!=0){
          $this.minu--;
          $this.second=59;
        }
        else{
          if($this.h!=0){
            $this.h--;
            $this.minu=59;
            $this.second=59;
          }
          else{
             if($this.day!=0){
              $this.day--;
              $this.h=23;
              $this.minu=59;
              $this.second=59;
            }
            else{
              alert("已截止");
              return;
            }
          }
        }
      }
      setTimeout(function(){
        $this.timedel()
      },1000);
    },
    gettime:function(){
      var $this = this;
      axios({
        method: "get",
        url: "/xier/niuniu/data.php"
      }).then(function(response) {
        let t=response.data.time-new Date().getTime();
        t=parseInt(t/1000);
        $this.day=parseInt(t/24/3600);
        $this.h=parseInt((t-24*3600*$this.day)/3600);
        $this.minu=parseInt((t-24*3600*$this.day-3600*$this.h)/60);
        $this.second=parseInt((t-24*3600*$this.day-3600*$this.h-60*$this.minu));
      });
      $this.timedel();
    },
    rev: function() {
      var $this = this;
      axios({
        method: "get",
        url: "/xier/niuniu/data.php"
      }).then(function(response) {
        $this.num1 = response.data[0].piao;
        $this.num2 = response.data[1].piao;
        $this.ques = response.data.main_s;
        $this.slogan1 = response.data[0].slogan;
        $this.slogan2 = response.data[1].slogan;
      });
      setTimeout(function() {
        $this.rev();
      }, 1000);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.index {
  width: 100%;
  height: 100%;
}
#headbar {
  width: 90%;
  position: relative;
  left: 5%;
  height: 20%;
  display: flex;
  img{
    height: 100%;
  }
  .data{
    font-size: 1.5vw;
    width: 100%;
    position: relative;
    top:25%;
    p{
      margin-left: 10%;
      font-weight: bold;
      display: inline-block;
      span{
        font-weight:normal;
      }
    }
  }
}
.team{
  width: 100%;
  height: 10%;
}
.time{
  width:100%;
  height: 20%;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  img{
    width: 8%;
    height: auto;
    position: relative;
  }
  p{
    height: 100%;
    display: inline;
    font-size: 3vh;
    display: flex;
    align-items: center;
    position: relative;
  }
}
.num1 {
  float: left;
  font-size: 50px;
  font-weight: bold;
  color: #e51c23;
}
.num2 {
  float: right;
  font-size: 50px;
  font-weight: bold;
  color: #3e50b5;
}
.num {
  display: inline-block;
  font-size: 50px;
  color: black;
  font-weight: bold;
  span{
    color: black;
    font-weight: normal;
  }
}
.team{
  width: 80%;
  position: relative;
  left: 10%;
  text-align: center;
}
#mid {
  width: 100%;
  height: 80%;
  .ques {
    border-top: 5px solid #f8f8f8;
    height: 10%;
    position: relative;
    top: 5%;
    margin-bottom: 3%;
    font-size: 8vh;
    text-align: center;
    color: #965e25;
    font-weight: bold;
  }
  .v {
    position: relative;
    width: 80%;
    height: 8%;
    left: 10%;
  }
  .voting {
    position: relative;
    left: 10%;
    width: 80%;
    height: 20%;
    border-radius: 100px;
    background-color: white;
    .leftv {
      display: inline-block;
      height: 100%;
      width: 100%;
      position: absolute;
      right: 0;
      border-radius: 100px;
      border-radius: 100px;
      background-color: #e51c23;
    }
    .rightv {
      display: inline-block;
      height: 100%;
      position: absolute;
      right: 0;
      border-top-right-radius: 100px;
      border-bottom-right-radius: 100px;
      background-color: #3e50b5;
      transition: width 1s;
    }
  }
}
#tail {
  width: 100%;
  height: 20%;
  background-color: white;
  position: absolute;
  bottom: 0;
  p {
    width: 100%;
    position: fixed;
    bottom: 0;
    text-align: center;
    font-size: 20px;
    color: #727272;
  }
}
</style>
