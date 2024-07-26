<template>  
  <div>  
    <div class="top">
      <span class="title">ToDoList</span>
      <input v-model="value" @keyup.enter="addTodo" placeholder="添加新事项"  class="addInput"/>  
      <button @click="addTodo">确定</button>
    </div>
    <div class="todolist">
      <h2  class="listTitle">
        "正在进行"
        <span class="listSpan">{{todos.length}}</span>
      </h2>
      <ol>  
        <li v-for="todo in todos" :key="todo.id" class="listItem">
          <input type="checkbox" @click="completed(todo.id,todo.name)">
          <input v-bind:value="todo.name" v-on:input="changeValue" :disabled="todo.status" type="text" :class="[todo.status?'input':'active']"/>  
          <a href="##" @click="handleDelete(todo.id)">删除</a>
          <a v-if="todo.status" href="##" @click="handleEdit(todo.id)">修改</a> 
          <a v-else href="##" @click="confirm(todo.id)">完成</a> 
        </li>  
      </ol> 
    </div>
    <div class="todolist">
      <h2  class="listTitle">
        "已经完成"
        <span class="listSpan">{{selected.length}}</span>
      </h2>
      <ol>  
        <li v-for="item in selected" :key="item.id" class="listItem">  
          <span>{{ item.name }}</span>  
        </li>  
      </ol> 
    </div>
  </div>  
</template>

<script setup>  
import { ref } from 'vue';  
// 初始化新的待办事项   
var currentValue=ref('');
var value='';
// 待办事项列表  
var todos = ref([{
  id:0,
  name:'test',
  status:true
}]);  
var selected = ref([]); 

// 添加新的事项  
function addTodo() { 
  var item={
    id:todos.value.length,
    name:value,
    status:true
  }
  todos.value.push(item)
  value='';

}  

function handleDelete(id){
  var test=todos.value.filter(item=>item.id!=id)
  todos=ref(test)
}
function handleEdit(id){
  todos.value.forEach(item => {
    if(item.id==id){
      item.status=false
    }
  })
}
function changeValue(event){
  currentValue=event.target.value
}
function confirm(id){
  todos.value.forEach(item => {
    if(item.id==id){
      if(currentValue!=''){
        item.name=currentValue
      }
      item.status=true
    }
  })
  currentValue='';
}
function completed(id,name){
  var newItem={
    id:selected.value.length,
    name:name
  }
  selected.value.push(newItem)
  handleDelete(id);
}
</script> 

<style scoped>  
.top{
  width: 100%;
  height: 50px;
  line-height: 50px;
  background-color: black;
}
.title{
  color:white;
  margin-left: 10px;
  float: left;
}
.addInput{
  width: 60%;height: 24px;border-radius: 5px;
}
.todolist{
  width: 60%;
}
.listItem{
  width: 100%;display: flex;justify-content: space-around;
}
.listTitle{
  display: flex;
}
.listSpan{
  margin-left: 20px;
}
.active{
  border: 1px solid;
  background-color: ghostwhite;
}
.input{
  border: 0px;background-color: white;
}
</style>
