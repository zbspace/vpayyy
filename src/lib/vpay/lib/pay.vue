<template>
  <div class="zfb-pay" v-if="show">
    <div class="content">
      <!--标题栏-->
      <header class="pay-title">
        <div class="ico-back" @click="cancel"></div>
        <h3>{{ title }}</h3>
      </header>

      <!--密码框-->
      <div class="pass-box">
        <ul class="pass-area">
          <li
            class="pass-item"
            :class="{ on: password.length > index }"
            v-for="(item, index) in digit"
            :key="index"
          ></li>
        </ul>
      </div>

      <!--忘记密码-->
      <div class="forget-pass">
        <div class="forget-pass-btn" @click="forget">忘记密码</div>
      </div>

      <!--键盘区-->
      <ul class="keyboard">
        <li @click="onKeyboard(1)">
          <p class="num"><strong>1</strong></p>
          <p class="character"></p>
        </li>
        <li @click="onKeyboard(2)">
          <p class="num"><strong>2</strong></p>
          <p class="character">ABC</p>
        </li>
        <li @click="onKeyboard(3)">
          <p class="num"><strong>3</strong></p>
          <p class="character">DEF</p>
        </li>
        <li @click="onKeyboard(4)">
          <p class="num"><strong>4</strong></p>
          <p class="character">GHI</p>
        </li>
        <li @click="onKeyboard(5)">
          <p class="num"><strong>5</strong></p>
          <p class="character">JKL</p>
        </li>
        <li @click="onKeyboard(6)">
          <p class="num"><strong>1</strong></p>
          <p class="character">MNO</p>
        </li>
        <li @click="onKeyboard(7)">
          <p class="num"><strong>7</strong></p>
          <p class="character">PQRS</p>
        </li>
        <li @click="onKeyboard(8)">
          <p class="num"><strong>8</strong></p>
          <p class="character">TUV</p>
        </li>
        <li @click="onKeyboard(9)">
          <p class="num"><strong>9</strong></p>
          <p class="character">WXYZ</p>
        </li>
        <li class="none"></li>
        <li class="zero" @click="onKeyboard(0)"><strong>0</strong></li>
        <li class="delete" @click="deleteKey"></li>
      </ul>

      <!--加载中状态-->
      <div class="loading-wrap" v-if="payStatus !== 0">
        <div class="loading">
          <!--加载图标-->
          <img
            src="./images/loading.png"
            class="loading-ico"
            alt=""
            v-if="payStatus === 1"
          />
          <img
            src="./images/success.png"
            class="success-ico"
            alt=""
            v-if="payStatus === 2"
          />
          <!--加载文字-->
          <p v-if="payStatus === 1">{{ loadingText }}</p>
          <p v-if="payStatus === 2">{{ finishedText }}</p>
        </div>
      </div>

      <!--支付失败提示框-->
      <div class="pay-fail" v-if="isShowFail">
        <div class="pay-fail-lay">
          <h3 class="title">{{ failTip }}</h3>
          <div class="btns">
            <div @click="reInput">重新输入</div>
            <div @click="forget">忘记密码</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts" src="./pay.ts"></script>

<style lang="scss">
ul,
li {
  list-style: none;
}
.zfb-pay {
  position: fixed;
  top: 0;
  bottom: 0;
  width: 100%;
  max-width: 750px;
  z-index: 998;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column-reverse;
}
.content {
  background: #fafafa;
  position: relative;
}
.content .pay-title {
  position: relative;
  height: 40px;
  line-height: 40px;
  border-bottom: 1px solid #e5e5e5;
}
.content .pay-title h3 {
  font-size: 18px;
  font-weight: normal;
  text-align: center;
}
.content .ico-back {
  position: absolute;
  left: 0;
  top: 0;
  width: 40px;
  height: 40px;
  background: url(./images/back.png) no-repeat center;
}
/* 密码框 */
.pass-box {
  padding: 0 20px;
}
.pass-area {
  display: flex;
  height: 40px;
  margin-top: 20px;
  border: 1px solid #e7e7e7;
  border-radius: 4px;
  background: #fff;
}
.pass-area .pass-item {
  position: relative;
  flex: 1;
  border-right: 1px solid #e7e7e7;
}
.pass-area .pass-item:last-child {
  border-right: 0;
}
.pass-area .pass-item.on::after {
  content: "";
  position: absolute;
  left: 50%;
  top: 50%;
  width: 8px;
  height: 8px;
  background: #000;
  border-radius: 50%;
  transform: translate(-50%, -50%);
}
/* 密码框 */
.forget-pass {
  padding: 10px 20px 0;
  text-align: right;
}
.forget-pass-btn {
  color: #0083e3;
}
/* 键盘区 */
.keyboard {
  display: flex;
  flex-wrap: wrap;
  margin-top: 65px;
  background: #fff;
}
.keyboard li {
  width: 33.3333%;
  height: 45px;
  padding-top: 4px;
  text-align: center;
  border-bottom: 1px solid #b2b2b2;
  border-right: 1px solid #b2b2b2;
  box-sizing: border-box;
  user-select: none;
}
.keyboard li .num {
  line-height: 1.2;
  font-size: 18px;
}
.keyboard li .character {
  line-height: 1.2;
  font-size: 12px;
}
.keyboard li:nth-child(3n) {
  border-right: 0;
}
.keyboard li:active {
  background-color: #d1d5db;
}
.keyboard .zero {
  border-bottom: 0;
  line-height: 44px;
  font-size: 18px;
  padding: 0;
}
.keyboard .none {
  border-bottom: 0;
  background: #d1d5db;
}
.keyboard .delete {
  border-bottom: 0;
  background: url(./images/delete.png) no-repeat center #d1d5db;
}
.keyboard .delete:active {
  border-bottom: 0;
  background-color: #fff;
}
/* 加载中 */
.loading-wrap {
  position: absolute;
  left: 0;
  top: 40px;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  background: #fff;
}
.loading-wrap .loading {
  text-align: center;
}
.loading-wrap .loading .loading-ico {
  animation: rotate 0.6s linear infinite;
}
.loading-wrap .loading p {
  margin-top: 6px;
}
@-webkit-keyframes rotate {
  0% {
    transform: rotate(0);
  }
  50% {
    transform: rotate(180deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
@keyframes rotate {
  0% {
    transform: rotate(0);
  }
  50% {
    transform: rotate(180deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
/* 支付失败弹窗 */
.pay-fail {
  position: fixed;
  top: 0;
  bottom: 0;
  width: 100%;
  max-width: 750px;
  background: rgba(0, 0, 0, 0.6);
  z-index: 999;
}
.pay-fail-lay {
  width: 60%;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background: #fff;
  border-radius: 10px;
  overflow: hidden;
}
.pay-fail-lay .title {
  line-height: 30px;
  text-align: center;
  font-size: 16px;
  padding: 10px;
}
.pay-fail-lay .btns {
  display: flex;
  border-top: 1px solid #f1f1f1;
}
.pay-fail-lay .btns div {
  flex: 1;
  height: 40px;
  line-height: 40px;
  text-align: center;
}
.pay-fail-lay .btns div:active {
  background: #f3f3f3;
}
.pay-fail-lay .btns div:last-child {
  border-left: 1px solid #f1f1f1;
}
</style>
