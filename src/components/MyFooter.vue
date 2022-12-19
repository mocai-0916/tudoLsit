<template>
    <div>
        <div class="todo-footer" v-if="total">
            <label>
                <!-- <input type="checkbox" :checked="isAll" @change="checkAll"/> -->
                <input type="checkbox" v-model="isAll"/>
            </label>
            <span>已完成{{ changTudo }}</span>/
            <span>全部{{ total }}</span>
            <button class="btn btn-danger" @click="cleartudo">清除已完成任务</button>
        </div>
    </div>
</template>

<script>
export default {
    props: ["tudos"],
    computed: {
        // doneTotal() {
        //     let index = 0;
        //     for (const i of this.tudos) {
        //         if (i.done) {
        //             index++;
        //         }
        //     }
        //     return index;
        // },
        changTudo(){
          //筛选
          return this.tudos.reduce((pre,tudo) => {
            return pre + (tudo.done ? 1 : 0)
          },0)
        },
        total(){
          return this.tudos.length
        },
        isAll:{
          get(){
            return this.changTudo === this.total && this.total > 0
          },
          set(value){ //set方法会接收到一个value
            return this.$emit('chackAll',value)
          }
        }
    },
    methods: {
      cleartudo() {
        // this.clearAllTudo()
        this.$emit('clearAllTudo')
      }
    },
};
</script>

<style scoped>
.todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
}

.todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
}

.todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
}

.todo-footer button {
    float: right;
    margin-top: 5px;
}
</style>