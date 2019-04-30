<template>
  <div class="pos">
    <el-row>
      <el-col :span="8" class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border show-summary style="width: 100%" class="table-list">
                <el-table-column prop="goodsName" label="商品"  ></el-table-column>
                <el-table-column prop="count" label="数量" width="50"></el-table-column>
                <el-table-column prop="price" label="金额(元)" width="70"></el-table-column>
                <el-table-column  label="操作" width="100" fixed="right">
                    <template slot-scope="scope">
                        <el-button type="text" size="small">删除</el-button>
                        <el-button type="text" size="small">增加</el-button>
                    </template>
                </el-table-column> 
            </el-table>
            <div class="div-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger">删除</el-button>
              <el-button type="success">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">
            挂单
          </el-tab-pane>
          <el-tab-pane label="外卖">
            外卖
          </el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="16">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="items in oftenGoods" @click="addOrderList(items)">
                <span>{{items.goodsName}}</span>
                <span class="o-price">￥{{items.price}}元</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class='cookList'>
                    <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div>
                <ul class='cookList'>
                    <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class='cookList'>
                    <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div>
                <ul class='cookList'>
                    <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                </ul>
              </div>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: "pos",
  data() {
    return {
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: []
    }
  },
  created: function(){
    axios.get("https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods",{})
    .then(response => {
      this.oftenGoods = response.data;
    })
    .catch(error => {
      alert("网络错误，不能访问");
    })

    axios.get("https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods",{})
    .then(response => {
      this.type0Goods = response.data[0];
      this.type1Goods = response.data[1];
      this.type2Goods = response.data[2];
      this.type3Goods = response.data[3];
    })
    .catch(error => {
      alert("网络错误，不能访问");
    })
  },
  // 所有虚拟DOM加载完成之后
  mounted: function() {
    let orderHeight = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderHeight + "px";
  },
  methods: {
    addOrderList(goods){
      // 商品是否存在于列表中
      let isHave = false;
      for(let i = 0; i < this.tableData.length; i++){
        if(this.tableData[i].goodsId == goods.goodsId){
          isHave = true;
        }
      }

      //判断值编写业务逻辑
      if(isHave){
        // 改变列表中商品数量
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        let oldPrice = goods.price;
        arr[0].count++;
        arr[0].price = oldPrice * arr[0].count;
      }else{
        let newGoods = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
        this.tableData.push(newGoods);
      }
    },
    deleteGoods(goods){

    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.pos-order {
  background: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.div-btn{
  margin-top: 10px;
}
.el-tabs__nav{
  width: 100%;
  display: flex;
}
.el-tabs__item{
  flex: 1;
}
table .cell{
  text-align: center;
}
.title{
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
}
.often-goods-list ul li{
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
  border-radius: 5px;
}
.o-price{
  color: #58b7ff;
}
.goods-type{
  clear: both;
}
.cookList li{
  list-style: none;
  width:22%;
  border:1px solid #E5E9F2;
  height: auto;
  overflow: hidden;
  background-color:#fff;
  padding: 5px;
  float:left;
  margin: 2px;
  position: relative;
}
.cookList li span{
  display: block;
  padding-left: 45%;
}
.foodImg{
  width: 40%;
  position: absolute;
  left: 5px;
  top: 5px;
  padding-left: 0!important;
}
.foodImg img{
  width: 100%;
  height: 60px;
}
.foodName{
  font-size: 16px;
  padding-left: 10px;
  color:brown;
  margin-top: 5px;
  overflow: hidden;
  white-space: nowrap; 
   text-overflow: ellipsis;
}
.foodPrice{
  display: block;
  font-size: 14px;
  padding-left: 10px;
  padding-top:10px;
  padding-bottom: 5px;
}
</style>
