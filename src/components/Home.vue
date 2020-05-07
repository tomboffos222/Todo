<template>
  <div class="container">
    <div class="row">

      <div class="col-lg-6 card">
        <Todo v-for="(todo,n) in todos" :todo="todo,n" />
      </div>

      <TodoForm/>
    </div>
  </div>
</template>

<script>
    import Todo from "./Todo";
    import TodoForm from "./TodoForm";

    export default {
      name: "Home.vue",
      components:{
        TodoForm,
        Todo
      },
      data(){
        return{
          todos:[],
        }
      },
      mounted() {
        if (localStorage.getItem('todos')){
          try{
            this.todos = JSON.parse(localStorage.getItem('todos'));

          }catch (e) {
            localStorage.removeItem('todos');

          }
        }
      },
      methods:{
        saveTodo(x){
          let parsed = JSON.stringify(x);
          localStorage.setItem('todos',parsed)
          this.todos = JSON.parse(localStorage.getItem('todos'))|| [];

        }
      }
    }
</script>

<style scoped>

</style>
