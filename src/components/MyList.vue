<template>
  <div>
    <ul class="todo-main">
      <transition-group
        appear
        enter-active-class="animate__animated animate__fadeIn"
        leave-active-class="animate__animated animate__fadeOut"
      >
        <my-item v-for="todo of tudos" :key="todo.id" :tit="todo"></my-item>
      </transition-group>
    </ul>
  </div>
</template>

<script>
import MyItem from "./MyItem.vue";
import "animate.css";
export default {
  components: { MyItem },
  props: ["tudos"],
};
</script>

<style scoped>
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0;
}
.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
.animate__animated.animate__fadeIn,
.animate__fadeOut {
  --animate-duration: 300ms;
}
/* .animate__animated.animate__fadeOut{
  --animate-duration: 5s;
} */
</style>

