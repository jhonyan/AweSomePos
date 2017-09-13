<template>
 <div class="left-nav">
   <ul>
     <li>
       <i class="icon iconfont icon-hongbaoiocn"></i>
       <div>收银</div>
     </li>
     <li>
       <i class="icon iconfont icon-dianpu"></i>
       <div>店铺</div>
     </li>
     <li>
       <i class="icon iconfont icon-meishi"></i>
       <div>商品</div>
     </li>
     <li>
       <i class="icon iconfont icon-mingxing"></i>
       <div>会员</div>
     </li>
     <li>
       <i class="icon iconfont icon-shujutongji"></i>
       <div>统计</div>
     </li>
   </ul>
 </div>
</template>

<script type="text/ecmascript-6">
export default{
  name: 'leftNav',
  data () {
    return {
    }
  }
}
</script>
<style>
 .left-nav{
  color: #ffffff;
  font-size: 10px;
  background-color: #1d8ce0;
  float: left;
  width: 5%;
  height: 100%;
 }
 .iconfont{
   font-size: 24px;
 }
 .left-nav ul{
   padding: 0px;
   margin: 0px;
 }
 .left-nav li{
   list-style: none;
   text-align: center;
   border-bottom: 1px solid #ccc;
   padding: 10px;
 }
</style>