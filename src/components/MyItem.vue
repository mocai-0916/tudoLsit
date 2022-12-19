<template>
  <li>
    <label>
      <input type="checkbox" :checked="tit.done" @change="chackID(tit.id)" />
      <!-- 如下代码也能实现功能，但是不太推荐，因为有点违反原则，因为修改了props -->
      <!-- <input type="checkbox" v-model="todo.done"/> -->
      <span v-show="!tit.isEdit">{{ tit.title }} </span>
      <input
        type="text"
        v-show="tit.isEdit"
        :value="tit.title"
        @blur="handlerBlur(tit, $event)"
        ref="inpfocus"
      />
    </label>
    <button class="btn btn-danger" @click="removeID(tit.id)">删除</button>
    <button class="btn btn-edit" @click="handlerEdit(tit)" v-show="!tit.isEdit">修改</button>
  </li>
</template>

<script>
import pubsub from "pubsub-js";
export default {
  props: ["tit"],
  methods: {
    chackID(val) {
      this.$bus.$emit("chackID", val);
    },
    removeID(id) {
      // this.$bus.$emit('removeID',id)
      pubsub.publish("removeID", id);
    },
    handlerEdit(tudo) {
      //Object.prototype.hasOwnProperty.call判断一个方法是否在该对象上
      if (Object.prototype.hasOwnProperty.call(tudo, "isEdit")) {
        tudo.isEdit = true;
        
      } else {
        this.$set(tudo, "isEdit", true);
      }
      //$nextTick在下一次dom更新完成后执行它的回调函数
      // this.$nextTick(function () {
      //   this.$refs.inpfocus.focus()
      // })
      //也可以写个定时器 因为定时器会在dom更新完才能执行
      // setTimeout(() => {
      //   this.$refs.inpfocus.focus()
      // },);
    },
    handlerBlur(tudo, e) {
      tudo.isEdit = false;
      if (!e.target.value.trim()) {
        return alert('输入不能为空')
      }
      this.$bus.$emit("updataTudo", tudo.id, e.target.value);
    },
  },
};
</script>

<style scoped>
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #ddd;
}

li:hover button {
  display: block;
}
</style>
