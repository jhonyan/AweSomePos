<template>
  <div class="Pos">
    <div>
      <el-row>
        <el-col :span='7' class="Pos-order" id='order-list'>
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data='tableData' border style="width:100%">
                <el-table-column prop='goodsName' label='商品'></el-table-column>
                <el-table-column prop='count' label='数量' width='70'></el-table-column>
                <el-table-column prop='price' label='金额' width='70'></el-table-column>
                <el-table-column label='操作' width='120' fixed='right'>
                  <template scope='scope'>
                    <el-button type='text' size='samll' @click="addOrdList(scope.row)">增加</el-button>
                    <el-button type='text' size='samll' @click="delSingleGood(scope.row)">删除</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div class="total-div">
                <small>数量：</small>{{totalCount}} &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp
                <small>总金额：</small>{{totalPrice}}元
              </div>
              <div class='operation'>
                <el-button type="warning">挂单</el-button>
                <el-button type="danger" @click="delAll()">删除</el-button>
                <el-button type="success" @click="checkOut()">结账</el-button>
              </div>
            </el-tab-pane>
            <el-tab-pane label="挂单">

            </el-tab-pane>
            <el-tab-pane label="外卖">

            </el-tab-pane>
          </el-tabs>
        </el-col>
        <el-col :span='17' class="ofen-goods">
          <div class="title">常用商品</div>
          <div class="goods-list">
            <ul>
              <li v-for="goods in oftenGoods" :key="goods.index" @click="addOrdList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <div>
                  <ul class="cook-list">
                    <li v-for="foods in type0Goods" :key="foods.index" @click="addOrdList(foods)">
                      <span class="food-img">
                        <img :src="foods.goodsImg" alt="foods.goodsName" width="100%">
                      </span>
                      <span class="food-name">{{foods.goodsName}}</span>
                      <span class="food-price">￥{{foods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <div>
                  <ul class="cook-list">
                    <li v-for="foods in type1Goods" :key="foods.index" @click="addOrdList(foods)">
                      <span class="food-img">
                        <img :src="foods.goodsImg" alt="foods.goodsName" width="100%">
                      </span>
                      <span class="food-name">{{foods.goodsName}}</span>
                      <span class="food-price">￥{{foods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <div>
                  <ul class="cook-list">
                    <li v-for="foods in type2Goods" :key="foods.index" @click="addOrdList(foods)">
                      <span class="food-img">
                        <img :src="foods.goodsImg" alt="foods.goodsName" width="100%">
                      </span>
                      <span class="food-name">{{foods.goodsName}}</span>
                      <span class="food-price">￥{{foods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <div>
                  <ul class="cook-list">
                    <li v-for="foods in type3Goods" :key="foods.index" @click="addOrdList(foods)">
                      <span class="food-img">
                        <img :src="foods.goodsImg" alt="香辣鸡腿堡" width="100%">
                      </span>
                      <span class="food-name">{{foods.goodsName}}</span>
                      <span class="food-price">￥{{foods.price}}元</span>
                    </li>
                  </ul>
                </div>
              </el-tab-pane>
            </el-tabs>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
import axios from 'axios'
export default {
  name: 'Pos',
  data() {
    return {
      tableData: [
      ],
      oftenGoods: [
      ],
      type0Goods: [
      ],
      type1Goods: [
      ],
      type2Goods: [
      ],
      type3Goods: [
      ],
      totalCount: 0,
      totalPrice: 0
    }
  },
  created: function() {
    axios.get('http://jspang.com/DemoApi/oftenGoods.php')
      .then(response => {
        this.oftenGoods = response.data
      })
      .catch(error => {
        alert("网络错误，不能访问!")
      }),

      axios.get('http://jspang.com/DemoApi/typeGoods.php')
        .then(response => {
          console.log(response)
          this.type0Goods = response.data[0];
          this.type1Goods = response.data[1];
          this.type2Goods = response.data[2];
          this.type3Goods = response.data[3];
        })
        .catch(error => {
          alert("网络错误，不能访问!")
        })
  },
  mounted: function() {
    var orderHelight = document.body.clientHeight
    document.getElementById('order-list').style.height = orderHelight + 'px'
  },
  methods: {
    addOrdList(goods) {
      //判断是否已经存在订单列表中
      let isHave = false
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true
        }
      }
      //根据判断结果编写业务代码
      if (isHave) {
        this.totalCount = 0
        this.totalPrice = 0
        //改变商品数量
        let arr = this.tableData.filter(g => g.goodsId == goods.goodsId)
        arr[0].count++
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        }
        this.tableData.push(newGoods)
      }
      this.getTotalPrice()
    },
    delSingleGood(goods) {
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId)
      this.getTotalPrice()
    },
    delAll() {
      this.tableData = []
      this.totalCount = 0
      this.totalPrice = 0
    },
    getTotalPrice() {
      this.totalCount = 0
      this.totalPrice = 0
      //计算总计
      this.tableData.forEach((element) => {
        this.totalCount += element.count
        this.totalPrice = this.totalPrice + (element.count * element.price)
      })
    },
    checkOut(){
      if(this.totalPrice != 0){
        this.tableData = [];
                this.totalCount = 0;
                this.totalMoney = 0;
                this.$message({
                    message: '结账成功，请继续努力!',
                    type: 'success'
                });
      }else{
        this.$message.error('不能空结，请确认有商品存在！');
      }
    }
  }
}
</script>
<style scoped >
.Pos-order {
  background-color: #f9fafc;
  border-bottom: 1px solid #ddd;
  border-right: 1px solid #666;
}

.operation {
  padding: 10px;
}

.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
}

.goods-list {
  overflow: hidden;
}

.goods-list,
.cook-list {
  cursor: pointer;
}

.goods-list>ul>li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 5px;
  background-color: #fff;
}

.o-price {
  color: #58b7ff;
}

.cook-list li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auto;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
}

.cook-list li span {
  display: block;
  float: left;
}

.food-img {
  width: 40%;
}

.food-name {
  font-size: 18px;
  padding: 10px 0 0 10px;
  color: brown;
}

.food-price {
  font-size: 16px;
  padding: 10px 0 0 10px;
}

.total-div {
  background-color: #fff;
  padding: 10px;
  border: 1px solid #e5e9f2;
}
</style>