<template>
    <div class="col-lg-6">
      <div class="card p-3 text-center">
        <h3>
          Создать заметку
        </h3>
        <div class="form-group">
          <input type="text" class="form-control" v-model="title" placeholder="Напишите заметку">

        </div>
        <div class="form-group">
          <input type="submit" value="Создать" @click="addTodo" class="btn btn-success form-control">
        </div>
      </div>
    </div>
</template>

<script>
    export default {
      name: "TodoForm",
      data() {
        return {
          title:null,
          todos:[],
          newTodo: {},
        }
      },

      methods:{
        addTodo(){
          if (!this.newTodo) return;
          this.newTodo = {
            "title": this.title,
            "checked": false
          };

          this.todos = JSON.parse(localStorage.getItem('todos')) || [];

          this.todos.push(this.newTodo);

          this.title = '';

          this.$parent.saveTodo(this.todos);


        },
      },

    }
</script>

<style scoped>

</style>
