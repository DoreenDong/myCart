<template>
    <div>
        <h2>我是购物车</h2>
        <table>
            <tr>
                <td>勾选</td>
                <td>课程名称</td>
                <td>课程价格</td>
                <td>数量</td>
                <td>价格</td>
            </tr>
            <tr v-for="(item,index) in courseItem" :key = item.id>
                <td>
                    <input type="checkbox" v-model="item.isActive">
                </td>
                <td>{{item.name}}</td>
                <td>{{item.price}}</td>
                <td>
                    <button @click="minQty(index)">-</button>
                    <input v-model="item.number">
                    <button @click="addQty(index)">+</button>
                </td>
                <td>{{item.number * item.price}}</td>
            </tr>
            <tr>
                <td ></td>
                <td colspan="2">{{isActiveCourse}}/{{allCourseList}}</td>
                <td colspan="2">{{allPrice}}</td>
            </tr>
        </table>
    </div>
</template>
<script>
export default {
    // 遵守单向数据流  需求需要修改父组件传过来的数据的时候需要用$emit来触发一下，父组件用$on来监听一下
    props:[
        'courseItem'
    ],
    data(){
        return{
            
        }
    },
    methods:{
        minQty(index){
            let number = this.courseItem[index].number;
            if(number>1){
                // 单向数据流
                this.courseItem[index].number -= 1;
            }else{
                if(window.confirm('确定要删除？')){
                    this.$emit('removeItem',index)
                }
            }
            
        },
        addQty(index){
            this.courseItem[index].number += 1;
        }
    },
    // 计算后的，用computed
    computed:{
        isActiveCourse(){
            return this.courseItem.filter(item=>item.isActive).length;
        },
        allCourseList(){
            return this.courseItem.length;
        },
        allPrice(){
            let num = 0;
            this.courseItem.forEach(item => {
                if(item.isActive){
                    num+=item.price*item.number
                }
            });
            return num;
        }
    }
}
</script>
<style>
    tr{
        border: 1px solid blue;
    }
    tr>td{
        border:1px solid purple;
    }
</style>