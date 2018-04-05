<template>
    <div class="left-nav">
        <ui>
            <li>
                <i class="icon iconfont icon-cangneishicao"></i>
                <div>收银</div>
            </li>

            <li>
                <i class="icon iconfont icon-chat"></i>
                <div>店铺</div>
            </li>

            <li>
                <i class="icon iconfont icon-car"></i>
                <div>商品</div>
            </li>

            <li>
                <i class="icon iconfont icon-businesscard"></i>
                <div>会员</div>
            </li>

            <li>
                <i class="icon iconfont icon-camera"></i>
                <div>统计</div>
            </li>
        </ui>
    </div>
</template>

<script>
export default {
  name: "LeftNav"
};
</script>

<style>
.left-nav {
  color: #fff;
  font-size: 10px;
  height: 100%;
  background-color: #1d8ce0;
  float: left;
  width: 5%;
}
.iconfont {
  font-size: 24px;
}
.left-nav ul {
  padding: 0px;
  margin: 0px;
}
.left-nav li {
  list-style: none;
  text-align: center;
  border-bottom: 1px solid #20a0ff;
  padding: 10px;
}
</style>


