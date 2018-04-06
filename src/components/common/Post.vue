<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span='7' class="pos-order" id="order-list">
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data="tableData" border show-summary style="width: 100%">
                <el-table-column prop="goodsName" label="商品" width="120"></el-table-column>
                <el-table-column prop="count" label="数量" width="80"></el-table-column>
                <el-table-column prop="price" label="金额" width="80"></el-table-column>
                <el-table-column label="操作" width="100" fixed="right">
                  <template scope='scope'>
                    <el-button type="text" @click="delSingleGoods(scope.row)" size="small">删除</el-button>
                    <el-button type="text" @click="checkout" size="small">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
            </el-tab-pane>
            <el-tab-pane label="挂单">
              挂单
            </el-tab-pane>
            <el-tab-pane label="外卖">
              外卖
            </el-tab-pane>
          </el-tabs>
        </el-col>
        <!--商品展示-->
        <el-col :span="17">
          <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">
              <ul>
                <li @click="addOrderList(goods)" v-for="goods in oftenGoods">
                  <span>{{goods.goodsName}}</span>
                  <span class="o-price">￥{{goods.price}}元</span>
                </li>
              </ul>
            </div>
            <div class="goods-type">
              <el-tabs>
                <el-tab-pane label="汉堡">
                  <ul class='cookList'>
                    <li @click="addOrderList(goods)" v-for="goods in type0Goods">
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </el-tab-pane>
                <el-tab-pane label="小食">
                  <ul class='cookList'>
                    <li @click="addOrderList(goods)" v-for="goods in type1Goods">
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </el-tab-pane>
                <el-tab-pane label="饮料">
                  <ul class='cookList'>
                    <li @click="addOrderList(goods)" v-for="goods in type2Goods">
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </el-tab-pane>
                <el-tab-pane label="套餐">
                  <ul class='cookList'>
                    <li v-for="goods in type3Goods">
                      <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                      <span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                  </ul>
                </el-tab-pane>
              </el-tabs>
            </div>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Post",
  data() {
    let a = {
      tableData: [],
      type0Goods: [],
      oftenGoods: []
    };
    return a;
  },
  methods: {
    checkout() {
      this.$message({
        message: "结账成功，感谢你又为店里出了一份力!",
        type: "success"
      });
    },
    //添加订单列表的方法
    addOrderList(goods) {
      this.totalCount = 0; //汇总数量清0
      this.totalMoney = 0;
      let isHave = false;
      //判断是否这个商品已经存在于订单列表
      for (let i = 0; i < this.tableData.length; i++) {
        console.log(this.tableData[i].goodsId);
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true; //存在
        }
      }
      //根据isHave的值判断订单列表中是否已经有此商品
      if (isHave) {
        //存在就进行数量添加
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        arr[0].count++;
        //console.log(arr);
      } else {
        //不存在就推入数组
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }

      //进行数量和价格的汇总计算
      // this.tableData.forEach(element => {
      //   this.totalCount += element.count;
      //   this.totalMoney = this.totalMoney + element.price * element.count;
      // });
    },
    delSingleGoods(goods) {
      console.log(goods);
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
    }
  },
  created() {
    axios
      .get("http://jspang.com/DemoApi/oftenGoods.php")
      .then(response => {
        console.log(response);
        this.oftenGoods = response.data;
      })
      .catch(error => {
        console.log(error);
        alert("网络错误，不能访问");
      });

    axios
      .get("http://jspang.com/DemoApi/typeGoods.php")
      .then(response => {
        console.log(response);
        //this.oftenGoods=response.data;
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
        this.type3Goods = response.data[3];
      })
      .catch(error => {
        console.log(error);
        alert("网络错误，不能访问");
      });
  },
  mounted: function() {
    var orderHeight = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderHeight + "px";
  }
};
</script>

<style scoped>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
</style>
<style>
#tab-0 {
  padding-left: 20px;
}
.title {
  border-bottom: 1px solid #d3dce6;
  height: 20px;
  background-color: #f9fafc;
  padding: 10px;
}
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  background-color: #fff;
  padding: 10px;
  margin: 5px;
}
.o-price {
  color: #58b7ff;
}
.goods-type {
  clear: both;
}
.cookList li {
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
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
</style>


