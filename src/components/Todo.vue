<template>
  <div class="  d-flex p-3 card flex-row mt-2 justify-content-around">
    <div class=" pt-1">
      <input type="checkbox" name="" class="form-check-input" :checked="todo.checked == true || todo.checked == 'true' "  disabled>
    </div>
    <div class="w-25 pt-1">
      <h6>

        {{todo.title}}
        <span v-if="todo.checked == true || todo.checked == 'true'">(Сделано)</span>
      </h6>
    </div>
    <div class="">
      <button class="btn btn-danger d-flex align-items-center justify-content-center" @click="$bvModal.show('bv_modal'+n)">
        <i class="material-icons">close</i>
      </button>

    </div>

    <div class="">
      <button class="btn btn-warning" @click="editTodo(n)">
        Изменить
      </button>
    </div>
    <b-modal  hide-footer v-bind:id="'bv_modal'+n" ref="'bv_modal'+n">
      <template v-slot:modal-title>

      </template>
      <div class="d-block text-center">
        <h3>Вы точно хотите удалить?</h3>
      </div>
      <b-button class="mt-3" block @click="$bvModal.hide('bv_modal'+n)">Отмена</b-button>
      <button class="w-100 btn btn-danger mt-1" @click="removeTodo(n)" >Удалить</button>
    </b-modal>
  </div>

</template>

<script>
    export default {
        name: "Todo",
        props:["todo","n"],
        data(){
          return{
            todos: []
          }
        },
        methods:{
          removeTodo(n){
            this.todos = JSON.parse(localStorage.getItem('todos'));
            this.todos.splice(n,1);
            this.$bvModal.hide('bv_modal'+n);
            this.$parent.saveTodo(this.todos)

          },
          checkTodo(n){
            this.todos = JSON.parse(localStorage.getItem('todos')) ||[];
            let todo = this.todos[n];
            todo.checked = true;
            this.$parent.saveTodo(this.todos);

          },
          editTodo(n){
            this.$router.push('/edit/'+n)

          }
        }
    }
</script>

<style scoped>

</style>
