<template>
  <div id="app">
    <h1>{{title}}</h1>
    <hr>
    <div>
      <h2>添加课程</h2>
      <div>
        <label for="">课程名称</label>
        <input type="text" v-model="courseInfo.name">
      </div>
      <div>
        <label for="">课程价格</label>
        <input type="text" v-model="courseInfo.price">
      </div>
      <div>
        <button @click="addCourseToList">添加课程到列表</button>
      </div>
    </div>
    <div>
      <h2>课程列表</h2>
      <table>
        <tr>
          <th>课程名称</th>
          <th>课程价格</th>
          <th>操作</th>
        </tr>
        <tr v-for="(item,index) in courseList" :key="item.id">
          <td>{{item.name}}</td>
          <td>{{item.price}}</td>
          <td><button @click="addCourseToCart(index)">添加课程到购物车</button></td>
        </tr>
      </table>
    </div>
    <cart :courseItem="courseItem" @removeItem="remove"></cart>
  </div>
</template>

<script>
import Cart from './components/Cart.vue';
export default {
  name: 'App',
  components:{
    Cart
  },
  data(){
    return{
      title:'购物车',
      courseInfo:{
        name:'',
        price:''
      },
      courseItem:[],
      courseList:[
        {
          id:0,
          name:'语文',
          price:200
        },{
          id:1,
          name:'数学',
          price:300
        }
      ]
    }
  },
  methods:{
    remove(index){
      this.courseItem.splice(index,1)
    },
    addCourseToList(){
      // debugger
      this.courseList.push(this.courseInfo)
    },
    addCourseToCart(index){
      let item = this.courseList[index];
      // 判断购物车的内容
      let isHasCourse = this.courseItem.find(x=>x.id == item.id)
      if(isHasCourse){
        isHasCourse.number += 1;
      }else{
        this.courseItem.push({
          ...item,
          number:1,
          isActive:true
        })
      }
    }
  }
}
</script>

<style>

</style>
