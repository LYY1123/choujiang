<template>
  <div id="appPage">
    <scroll>
      <!-- 头部 -->
      <div class="header">
        智慧生活&nbsp;&nbsp;&nbsp;未来城市的推动人      
      </div>
      <!-- 头部end -->
      <!-- 获得奖品展示 -->
      <div class="jpShow">
        <span> 已获得奖品：</span><span class="jpShowName">{{jpShowName}}</span>
      </div>
      <!-- 获得奖品展示end -->
      <!-- 转盘部分 -->
      <div class="zp-wrapper">
        <img src="http://hyw4445760001.my3w.com/choujiang/img/zhuanpan.png" alt=""
            :style="moveStyles"
            @load="imgLoad">
        <img src="http://hyw4445760001.my3w.com/choujiang/img/zhizhen.png" alt=""
            class="zhizhen"
            @click="startTurn">
      </div>
      <!-- 转盘部分end -->
      <!-- 规则说明 -->
      <div class="rule-wrapper">
        <p>抽奖规则：</p>
        <p>1.关注智慧生活公众号的用户，抽奖有效；未关注智慧生活公众号的用户，抽奖后关注了智慧生活公众号，抽奖有效；未关注智慧生活公众号的用户，抽奖后未关注智慧生活公众号的用户，抽奖无效。</p>
        <p>2.每个关注智慧生活公众号的用户只有一次抽奖机会。</p>
        <p>3.优惠券奖品仅可抵购智慧生活的智能门锁产品，智能门锁之外的其他产品不可使用优惠券。</p>
        <p>4.除优惠券之外的其他的奖品均需要到店领取。</p>
        <p>5.活动最终解释权归智慧生活所有。</p>
      </div>
      <!-- 规则说明end -->
      <!-- 店铺信息 -->
      <div class="storeMsg">
        <p>地址：兰考县健康南路未来都市26-8</p>
        <p>联系电话：15515513450（微信同）</p>
      </div>
      <!-- 店铺信息end -->
      <!-- 奖品弹框 -->
      <div class="jpModel-wrapper"
          v-if="jpModelShowFlag">
        <div class="jpModel">
          <p>{{jsModelTitle}}</p>
          <p class="jpName">{{jpName}}</p>
          <p class="jpModel-button"
            @click="jpModelConfirm">确定</p>
        </div>
      </div>
      <!-- 奖品弹框end -->
    </scroll>
    <loading :loadingAlreadyFlag="loadingAlreadyFlag"></loading>
  </div>
</template>
<script>
import scroll from "components/scroll.vue"
import loading from "components/loading.vue"
export default {
  name: 'AppPage',
  components: {
    scroll,
    loading
  },
  data () {
    return {
      moveStyles: {
        transition: 'all 0s',
        transform: 'translate(-50%,-50%) rotate(0deg)'
      },
      startTurning: 0,
      jpModelShowFlag: 0,
      jsModelTitle: '恭喜您获得以下奖品：',
      jpName: '',
      jpShowName: '您还未参与过抽奖',
      loadingAlreadyFlag: 0
    };
  },
  created () {
    let _this = this;
    if(localStorage.getItem('ZHSHJPNAME')){
      _this.jpShowName = localStorage.getItem('ZHSHJPNAME');
    }
  },
  mounted () {
  },
  methods: {
    startTurn () {
      let _this = this;
      if(_this.startTurning === 1) {
        return;
      }
      _this.startTurning = 1;
      _this.moveStyles.transition = 'all 0s';
      _this.moveStyles.transform = 'translate(-50%,-50%) rotate(0deg)';
      setTimeout(function(){
        // 判断是否参与过抽奖 
        // 已参数抽奖旋转固定圈数后提醒已经参数过
        // 并显示上次获得奖品
        if(localStorage.getItem('ZHSHZPFLAG') && localStorage.getItem('ZHSHZPFLAG') == 1 && localStorage.getItem('ZHSHJPNAME')) {
          _this.moveStyles = {
            transition: 'all 4s',
            transform: 'translate(-50%,-50%) rotate(1800deg)'
          }
          setTimeout(function() {
            _this.startTurning = 0;
            _this.jsModelTitle = "您已经参与过抽奖"
            _this.jpName = localStorage.getItem('ZHSHJPNAME');
            _this.jpModelShowFlag = 1;
          },4050)
        }else{
          // 计算中奖奖品所在度数
          _this.countJp();
        }
      },100)
      
      
    },
    countJp () {
      let _this = this;
      let probability = Math.ceil(Math.random()*100);
      if(probability <= 50) {
        _this.moveStyles = {
          transition: 'all 5s',
          transform: 'translate(-50%,-50%) rotate(2130deg)'
        }
        _this.jpName = "50元 优惠券"
      }
      if( probability > 50 && probability <= 75) {
        _this.moveStyles = {
          transition: 'all 5s',
          transform: 'translate(-50%,-50%) rotate(1950deg)'
        }
        _this.jpName = "80元 优惠券"
      }
      if(probability > 75 && probability <= 100) {
        _this.moveStyles = {
          transition: 'all 5s',
          transform: 'translate(-50%,-50%) rotate(2010deg)'
        }
        _this.jpName = "智能铅笔"
      }
      setTimeout(function() {
        _this.startTurning = 0;
        _this.jsModelTitle = "恭喜您获得以下奖品："
        _this.jpModelShowFlag = 1;
        _this.jpShowName = _this.jpName;
        localStorage.setItem('ZHSHZPFLAG',1);
        localStorage.setItem('ZHSHJPNAME',_this.jpName);
      },5050)
    },
    jpModelConfirm () {
      let _this = this;
      _this.startTurning = 0;
      _this.moveStyles.transition = 'all 0s';
      _this.moveStyles.transform = 'translate(-50%,-50%) rotate(0deg)';
      _this.jpModelShowFlag = 0;
    },
    imgLoad () {
      this.loadingAlreadyFlag = 1;
    }
  }
};
</script>
<style lang="less" scoped>
#appPage {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  width: 100%;
  height: 100%;
  background-image: url("http://hyw4445760001.my3w.com/easylife/img/content-bg.jpg");
  background-size: 100% 100%;
  overflow: hidden;
  .header {
    box-sizing: border-box;
    padding: 0.5rem 1rem;
    width: 100%;
    font-size: 1rem;
    color: #fff;
    line-height: 1.5;
    text-align: center;
  }
  .jpShow {
    font-size: 0.9rem;
    color: #fff;
    text-align: center;
    .jpShowName {
      color: #ffee51;
    }
  }
  .zp-wrapper {
    box-sizing: border-box;
    padding: 0.5rem 1rem;
    position: relative;
    width: 100%;
    height: 17rem;
    img {
      display: block;
      position: absolute;
      top: 50%;
      left: 50%;
      width: 80%;
      height: auto;
      transition: all 0s;
      transform: translate(-50%,-50%) rotate(0deg);
    }
    img.turn1 {
      transition: all 2s ease;
      transform: translate(-50%,-50%) rotate(-720deg);
    }
    img.zhizhen {
      top: 47%;
      width: 5rem;
      height: auto;
    }
  }
  .rule-wrapper {
    box-sizing: border-box;
    padding: 0.5rem 1rem;
    color: #fff;
    line-height: 1.5;
    p {
      font-size: 0.6rem;
    }
  }
  .storeMsg {
    box-sizing: border-box;
    padding: 0 1rem;
    width: 100%;
    font-size: 0.7rem;
    color: #fff;
    text-align: left;
  }
  .jpModel-wrapper {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.6);
    .jpModel {
      box-sizing: border-box;
      position: absolute;
      left: 50%;
      top: 50%;
      display: flex;
      flex-flow: column;
      align-items: center;
      justify-content: space-around;
      width: 80%;
      height: 8rem;
      font-size: 0.8rem;
      color: #fff;
      background-image: url("http://hyw4445760001.my3w.com/easylife/img/content-bg.jpg");
      background-size: 100% auto;
      border: 2px solid #ccc;
      transform: translate(-50%,-50%);
      .jpName {
        font-size: 1rem;
        color: #ffee51;
      }
      .jpModel-button {
        padding: 0.4rem 0;
        width: 70%;
        color: #fff;
        text-align: center;
        background-color: #088ef0;
      }
    }
  }
  
}
</style>
