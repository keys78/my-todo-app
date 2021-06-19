<template>
  <div class="apple">
      <AddTask />
  </div>
</template>

<script>
import AddTask from './components/AddTask.vue'

export default {
   props:['mode'],
   name: 'App',
   components: {
     AddTask
   }
  
}
</script>

<style>
*{
    padding:0;
    margin:0;
    box-sizing: border-box;
}
.josefin{
    font-family: 'Josefin Sans', sans-serif;;
}
@media screen and (max-width: 540px) {
   .remove-icon {
    display:block;
}
.mylists{
  font-size: 14px;
}
 .options-area{
   display: none;
  /* display: grid;
  grid-template-columns: 1fr 1fr;
  justify-content: center;
  align-items: center; */
}
/*
.btn-control{
  order:0;
  background: red;
  width:100%;
  margin:0 auto;
  grid-column: span 2;
}
.cleared{
  width:50%;
  order:0;
  background: green;
}
.todo-length{
  width:50%;
  order:0;
  background: rgb(255, 177, 10);
} */
.temp-holder{
   display: block;
}
.dark .options-area{
  background: white;
}
}


</style>