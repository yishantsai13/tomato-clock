<template>
  <div class="tomato">
    <div class="tomato-body">
      <div class="tomato-eye tomato-eye-left"></div>
      <div class="tomato-eye tomato-eye-right"></div>
      <!-- <div class="tomato-smile"></div> -->
      <img src="../assets/smile.png" alt="" class="tomato-smile">
      <div class="time">25:00</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Tomato',
  props:{
    tiem:{
      type: String,
    },
    status:{
      type: String,
    }
  }
}
</script>

<style lang="scss">
$tomato-height: 600px;
$tomato-width: 800px;

.tomato-body {
  height: $tomato-height;
  width: $tomato-width;
  background:$color-red;
  box-shadow: 0px 3px 6px #00000029;
  opacity: 1;
  border-radius: 60%;
  position: relative;
  margin-top: 50px;
}
.tomato-body::after {
  content: "";
  width: 70px;
  height: 135px;
  background:$color-grean;
  opacity: 1;
  border-radius: 90%;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
}
.tomato-body::before {
  content: "";
  width: 220px;
  height: 50px;
  background: $color-grean;
  opacity: 1;
  border-radius: 90%;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
}
.tomato-eye {
  width: 45px;
  height: 45px;
  background: $color-black;
  opacity: 1;
  border-radius: 50%;
}
.tomato-eye-left {
  position: absolute;
  top: 10%;
  left: 35%;
}
.tomato-eye-right {
  position: absolute;
  top: 10%;
  left: 60%;
}
.tomato-smile {
  // width: 30px;
  // height: 10px;
  // background: transparent;
  // border: 10px solid #333333;
  // border-bottom-left-radius: 10px;
  // border-bottom-right-radius: 10px;
  // border-top-left-radius: 4px;
  // border-top-right-radius: 4px;
  position: absolute;
  // background-color: #333333;
  top: 25%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.time{
  font-size: 172px;
  color: #FFFFFF;
  position: absolute;
  left: 50%;
  top: 60%;
  transform: translate(-50%, -50%);
}
</style>