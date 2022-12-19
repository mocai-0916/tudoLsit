<template>
  <div class="box">
    <div class="todo-container">
      <div class="todo-wrap">
        <!-- 把一个方法传给儿子inputbox -->
        <input-box></input-box>
        <my-list :tudos="tudos"></my-list>
        <my-footer :tudos="tudos" @clearAllTudo="clearAllTudo" @chackAll="chackAll"></my-footer>
      </div>
    </div>
  </div>
</template>

<script>
import inputBox from './components/inputBox.vue';
import MyFooter from './components/MyFooter.vue';
import MyList from './components/MyList.vue';
import pubsub from 'pubsub-js';
export default {
  components: { inputBox, MyList, MyFooter },
  methods: {
    receiveData(_, tudoObj) {
      this.tudos.unshift(tudoObj); //拿到input传来的数据直接unshift添加进数组里
    },
    chackID(id) {
      this.tudos.forEach(td => {
        if (td.id === id) {
          td.done = !td.done;
        }
      });
    },
    removeID(_, value) {
      //返回一个新数组 记得赋值再返回
      console.log(123);
      this.tudos = this.tudos.filter(todo => {
        return todo.id !== value;
      });
    },
    chackAll(value) {
      this.tudos.forEach(tudo => {
        tudo.done = value;
      });
    },
    clearAllTudo() {
      this.tudos = this.tudos.filter(tudo => {
        return !tudo.done;
      });
    },
    updataTudo(id, title) {
      this.tudos.forEach(t => {
        if (t.id === id) {
          t.title = title;
        }
      });
    }
  },
  watch: {
    //开启深度监听才能在tudos里发生变化时监测得到
    tudos: {
      deep: true,
      handler(newValue) {
        sessionStorage.setItem('tudos', JSON.stringify(newValue));
      }
    }
  },
  data() {
    return {
      tudos: JSON.parse(sessionStorage.getItem('tudos')) || []
    };
  },
  mounted() {
    this.$bus.$on('chackID', this.chackID);
    // this.$bus.$on('removeID',this.removeID);
    this.pubRemoveID = pubsub.subscribe('removeID', this.removeID);
    this.pubDataID = pubsub.subscribe('receiveData', this.receiveData);
    this.$bus.$on('updataTudo', this.updataTudo);
  },
  beforeDestroy() {
    //解绑pubsub
    pubsub.unsubscribe(this.pubRemoveID);
    pubsub.unsubscribe(this.pubDataID);
    //解绑事件总线
    this.$bus.$off('chackID');
    this.$bus.$off('updataTudo');
  }
};
</script>

<style>
body {
  background: #fff;
}
.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}
.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}
.btn-edit {
  color: #fff;
  background-color: #1456e6;
  border: 1px solid #207bc0;
}
.btn-edit-confirm {
  color: #fff;
  background-color: #000000;
  border: 1px solid #000000;
}
.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}
.btn:focus {
  outline: none;
}
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
