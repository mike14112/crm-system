<script setup lang="ts">
import { reactive, ref } from 'vue';
const toastmessage = ref<string>('')
const isShow =  ref<boolean>(false)
interface ITodo {
  title:string,
  isDone:boolean
}
const todo = reactive<ITodo>({
  title:'',
  isDone:false
})

const createTodo = ():void  =>  {
if(todo.title.length >= 2 && todo.title.length < 64){

 todo.title = todo.title;
   fetch ('https://easydev.club/api/v2/todos', {
   method:'POST',
   headers:{
      'Content-Type': 'application/json'
   },
   body:JSON.stringify(todo)
 })
 isShow.value = true
  toastmessage.value = 'ваш заметка добавлена '
 setTimeout(() => {
 todo.title = ''
isShow.value = false
toastmessage.value = ''
 }, 2000)
}else{
    isShow.value = true
    toastmessage.value = 'чтобы добавить заметку надо минимум 2 символа '
    setTimeout(() => {
    isShow.value = false
    toastmessage.value = ''

 }, 2000)

}
}


</script>

<template>
 <form @submit.prevent="createTodo" class="form">
  <label for="todo">Todo</label>
  <div class="input-area">
    <input type="text" v-model="todo.title" placeholder="to be text here.."  id="todo">
    <span v-show="isShow " class="text-error"> {{ toastmessage }}</span>
  </div>
<button type="submit" class="btn-add">Add</button>
 </form>
</template>



<style scoped>
.form{
  display: flex;
  align-items: center;
  gap: 2rem;
}

.input-area{
   display: flex;
   flex-direction: column;
}
.text-error{
  position: absolute;
   top: 15px;
   right: 20px;
   color: red;
   background-color: rgb(0, 255, 191);
   padding: 15px;
}
#todo{
  outline:none;
}
#todo:focus{
  outline: none;
}

.btn-add{
  cursor: pointer;
  color: white;
  font-weight: 900;
  background-color: rgb(30, 131, 165);
  width: 100px;
  padding: 15px;
  border-radius: 15px;
  border: none;
  outline: none;
}
.btn-add:hover{
  background-color: rgb(68, 148, 175);
  font-size: 15px
}
</style>
