<template>
  <div class="my-header">
    <div class="icon">
    </div>
    <h1 class="text">我的地盘！听我的</h1>
   <!-- 个人中心 -->
    <router-link to="/user" class="mine" tag="div">
      <i class="icon-mine"></i>
    </router-link>
  </div>
</template>

<script>
export default {
  name: "mheader"
};
</script>

<style lang="less" scoped>
@import "~@/common/less/const.less";
@import "~@/common/less/mymixin.less";

.my-header {
  position  : relative;
  height    : 44px;
  text-align: center;
  color     : @color-theme;
  font-size : 0;
  .icon {
    display: inline-block;
    .bg-image("logo");
    vertical-align : top;
    margin-top     : 6px;
    width          : 30px;
    height         : 32px;
    background-size: 30px 32px;
  }
  .text {
    display       : inline-block;
    vertical-align: top;
    line-height   : 44px;
    font-size     : @font-size-large;
  }
  .mine {
    position: absolute;
    top     : 0;
    right   : 0;
    .icon-mine {
      display  : block;
      padding  : 12px;
      font-size: 20px;
      color    : @color-theme;
    }
  }
}
</style>
