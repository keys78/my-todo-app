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
   padding:10px 8px;
}
 .options-area{
    grid-template-columns: repeat(6, 1fr);
    align-items: center;
    background:transparent;
    margin-top:-20px;

}
 

  .btn-control{
    grid-column: span 6;
    margin: 0 auto;
    order: 1;
    background: hsla(235, 24%, 19%);
    transition: background 0.5s ease-in-out;
    width: 100%;
    margin-top:12px;
    padding:10px 100px;
    border-radius: 4px;
  }
  .dark .btn-control  {
    color:hsl(235, 21%, 11%);
    background: rgb(244, 249, 255);
    transition: background 0.5s ease-in-out;
  }
  .dark .cleared-grp{
    color:hsl(235, 21%, 11%);
    background: rgb(244, 249, 255);
    transition: background 0.5s ease-in-out;
  }

  .cleared-grp{
    grid-column: span 3;
    padding:16px ;
    background: hsla(235, 24%, 19%);
    transition: background 0.5s ease-in-out;
  }


  .todo-length{
    grid-column: span 3;
    padding:16px ;
    background: hsla(235, 24%, 19%);
    transition: background 0.5s ease-in-out;

  } 

  .dark .todo-length {
    color:hsl(235, 21%, 11%);
    background: rgb(244, 249, 255);
    transition: background 0.5s ease-in-out;
  } 

  .temp-holder{
    display: block;
  }

  .dark .options-area{
    background: white;
  }
  .top-holder{
      height: 350px;
  }
}


</style>