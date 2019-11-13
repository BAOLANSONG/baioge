<template>
  <div class="home">
    <div class="leftDiv">
      <p>概要</p>
      <p>banner管理</p>
      <p>banner列表</p>
      <p>商品管理</p>
      <p>订单/售后</p>
      <p>用户管理</p>
      <p>供应商管理</p>
      <p>运营中心</p>
      <p>客服中心</p>
    </div>
    <div class="rightDiv">
      <div class="topDiv">
        标签筛选：
        <input type="text" />
        类型筛选：
        <select name="" id="">
          <option value="全部"></option>
        </select>
        时间筛选：
        <input type="text" placeholder="年/月/日" /> ~
        <input type="text" placeholder="年/月/日" />
        <p> <button  class='pp'>筛选</button><button  class='pp'>重置</button></p>
      </div>
      <div class="botmDiv">
        <button class='pp'  @click="addFn">创建新的banner</button>
        <table cellspacing="0" cellpadding="0">
          <tr>
            <th>序号</th>
            <th>姓名</th>
            <th>地址</th>
            <th>排序</th>
            <th>创建时间</th>
            <th>操作</th>
          </tr> 
          <tr v-for="(item,index) in list" :key="index">
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.ider}}</td>
            <td>{{item.sort}}</td>
            <td>2019/11/13</td>
            <td><button @click="bianji(item.id)">编辑</button>  <button @click="deletefn(item.id)">删除</button>
            </td>
            
          </tr>
        </table>
        <div class="mark" v-show="mark">
            <p>用户名：<input type="text" placeholder="请输入用户名" v-model="name"></p>
            <p>密码: <input type="text" placeholder="请输入密码"  v-model="password"></p>
            <p>地址: <input type="text" placeholder="请输入地址"  v-model="ider"></p>
            <p>号码：<input type="text" placeholder="请设置号码"  v-model="sort"></p>
            <button  @click="queren">确认</button> <button  @click="quxiao">取消</button>
        </div>
        <div class="block" >
          <el-pagination
            @current-change="indexFn"
            layout="prev, pager, next"
            :total="lengths"
            :page-size="limit"
            >
          </el-pagination>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "../style.scss"
import ElementUI from 'element-ui';
import axios from 'axios';
export default {
  name: "home",
  components: {},
  data(){
    return {
      list:[],
      mark:false,
      name:"",
      password:"",
      ider:"",
      sort:"",
      limit:4,
      lengths:5
    }
  },
  created(){
    axios.get("/api/fen").then(res=>{
      const data = res.data.data.data
      this.list=data
    })
  },
  methods:{
    deletefn(id){
      axios.get(`/api/delete?id=${id}`).then(res=>{
          axios.get("/api/fen").then(res=>{
            const data = res.data.data.data
            this.list=data
          })
      })
    },
    addFn(){
      this.mark=true
    },
    quxiao(){
      this.mark=false
    },
    queren(){
      this.mark=false
      console.log(this.name,this.password,this.ider,this.sort)
       axios.get(`/api/add?name=${this.name}&password=${this.password}&ider=${this.ider}&sort=${this.sort}`).then(res=>{
            axios.get("/api/list").then(res=>{
                const data = res.data.data.data
                this.list=data
              })
        })
    },
    indexFn(index){
      console.log(index)
      axios.get(`/api/fen?index=${index}&limit=${this.limit}`).then(res=>{
        this.list=res.data.data.data
        this.lengths=res.data.lengths
        console.log(this.lengths)
      })
    },
    bianji(id){ 
      this.mark=true
      axios.get(`/api/listfn?id=${id}`).then(res=>{
        console.log(res.data.data.data)
        const Ress = res.data.data.data
        this.name=Ress.name
        this.password = Ress.password
        this.ider = Ress.ider
        // this.sort = Ress.sort
      })
    }
  }
};  
</script>
