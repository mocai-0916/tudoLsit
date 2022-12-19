<template>
  <div>
    <input type="text" placeholder="输入你的任务名称，按回车键确认" @keydown.enter="update" v-model="val">
  </div>
</template>

<script>
import pubsub from 'pubsub-js';
import {nanoid} from "nanoid";
  export default {
    data() {
      return {
        val:''
      }
    },
    methods: {
      update() {
        // console.log(e.target.value);  传入event 使用原生方法e.target.value
        // console.log(this.val);  //或者使用双向数据绑定
        //校验数据
        if (!this.val.trim()) return alert('输入不能为空')  //!this.val 取反是因为空字符串为false 要为true才能执行return
        //将用户输入包装成一个tudo对象
        const tudoObj = {id:nanoid(),title:this.val,done:false}
        // this.receiveData(tudoObj) //将要添加的事件以实参的方式传给父亲app
        // this.$emit('receiveData',tudoObj)
        pubsub.publish('receiveData',tudoObj)
        this.val = ''
      }
    },
    //接收父亲app传来的一个方法

  }
</script>


<style scoped>
  input{
    width: 540px;
    height: 25px;
    border: 1px solid #66afe9;
    border-radius: 10px;
    outline:none;
    padding: 0 20px;
  }
  input:focus{
    border-color: #66afe9;
    outline: 0;
    -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075),0 0 8px rgba(102,175,233,.6);
    box-shadow: inset 0 1px 1px rgba(0,0,0,.075),0 0 8px rgba(102,175,233,.6)
}
</style>