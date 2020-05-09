<template>
  <div class="container">
    <div class="row">

      <div class="col-lg-12 card  p-3 ">
        <div class="d-flex justify-content-between">
          <button class=" btn btn-success mb-4 w-25 " @click="goBack()">
            Назад
          </button>
          <button class="btn btn-danger w-25 mb-4" @click="$bvModal.show('delete-modal')">
            Удалить
          </button>
        </div>
        <div class="form-group">
          Сделано: <input type="checkbox" :checked="this.checked == true" v-model="checked"  @click="checkEdit">
        </div>
        <div class="form-group">
          <input type="text" class="form-control" v-model="title">

        </div>
        <div class="form-group">
          <input type="submit" value="Изменить" class="btn btn-warning" @click="$bvModal.show('edit-modal')">
          <input type="submit" value="Отменить редактирование" class="btn btn-danger" @click="cancelEdited()">
          <input type="submit" value="Отменить изменение" class="btn btn-primary" @click="cancelSubmitted()">
          <input type="submit" value="Вернуть" class="btn btn-success" v-if="returnButton" @click="returnString()">
        </div>
      </div>
    </div>
    <b-modal id="delete-modal" hide-footer ref="delete-modal">
      <template v-slot:modal-title>

      </template>
      <div class="d-block text-center">
        <h3>Вы точно хотите удалить?</h3>
      </div>
      <b-button class="mt-3" block @click="$bvModal.hide('delete-modal')">Отмена</b-button>
      <button class="w-100 btn btn-danger mt-1" @click="deleteModal" >Удалить</button>
    </b-modal>
    <b-modal id="edit-modal" hide-footer ref="edit-modal">
      <template v-slot:modal-title>

      </template>
      <div class="d-block text-center">
        <h3>Вы точно хотите изменить?</h3>
      </div>
      <b-button class="mt-3" block @click="$bvModal.hide('edit-modal')">Отмена</b-button>
      <button class="w-100 btn btn-warning mt-1" @click="editTodo" >Изменить</button>
    </b-modal>
  </div>

</template>

<script>

  let todos = JSON.parse(localStorage.getItem('todos'));
    export default {

      name: "Edit.vue",
      data(){

        return{
          todo:todos[this.$route.params.id],
          title:'',
          returnButton:false,
          checked:todos[this.$route.params.id].checked,
        }
      },
      created: function () {
        let todos = JSON.parse(localStorage.getItem('todos'));
        this.todo=todos[this.$route.params.id];

        localStorage.setItem('now_'+this.$route.params.id,this.todo.title);
        localStorage.setItem('now_checked_'+this.$route.params.id,todos[this.$route.params.id].checked);
        this.title = this.todo.title;
        this.checked = this.todo.checked;

      },
      methods:{
        checkEdit(){

          this.checked = this.checked !== true;
        },
        goBack(){
          localStorage.removeItem('now_'+this.$route.params.id);
          localStorage.removeItem('now_checked_'+this.$route.params.id);
          localStorage.removeItem('last_'+this.$route.params.id);
          localStorage.removeItem('last_checked_'+this.$route.params.id);
          this.$router.go(-1)
        },
        cancelEdited(){


          this.title = this.todo.title

        },

        saveTodo(x){
          let parsed = JSON.stringify(x);
          localStorage.setItem('todos',parsed);
          this.todos = JSON.parse(localStorage.getItem('todos'))|| [];
        },
        editTodo(){
          this.todo=todos[this.$route.params.id];

          this.todo.title = this.title;
          this.todo.checked = this.checked;

          this.saveTodo(todos);
          this.$refs['edit-modal'].hide();
        },
        cancelSubmitted(){

          this.title = localStorage.getItem('now_'+this.$route.params.id);


          localStorage.setItem('last_' + this.$route.params.id, this.todo.title);
          localStorage.setItem('last_checked_'+this.$route.params.id,this.checked);

          this.returnButton = 1;
          this.checked = localStorage.getItem('now_checked_' + this.$route.params.id) === 'true';
        },
        returnString: function () {

          this.title = localStorage.getItem('last_' + this.$route.params.id);


          this.returnButton = false;
          this.checked = localStorage.getItem('last_checked_' + this.$route.params.id) === 'true';
        },
        deleteModal(){
          todos.splice(this.$route.params.id,1);
          this.saveTodo(todos);
          this.$refs['delete-modal'].hide();
          this.$router.push('/');

        }

      }

    }
</script>

<style scoped>

</style>
