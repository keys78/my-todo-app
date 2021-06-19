<template>
  <div class="hero h-screen" :class="mode">
    <div class="imgHolder h-72 w-full"> </div>

   
    <div class="xl:w-5/12 lg:w-6/12 md:w-7/12 sm:w-9/12 w-11/12 mx-auto -mt-60">
        <div class="my-header">
            <h1>TODO</h1>
            <NightModeButton :mode="mode" @nightMode="nightMode"/>
        </div>
        <form class="" @submit.prevent="addTodo" >
            <div class="input-group rounded flex gap-8 items-center ">
                <div class="sm:ml-5 ml-2 h-5 w-5 border border-gray-500 bg-transparent rounded-full"></div>
                <input v-model="myTodos" placeholder="Create a new todo..." type="text" class=" border-none bg-transparent focus:outline-none"/>
            </div>
        </form>

        <div class="py-10 text-white top-holder-x">
            <div class="text-white top-holder">
            <ul v-for="(todo, index) in filteredTodos" :key="index" class="list-container">
                <li class="mylists rounded">
                    <div class="w-1/12 sm:pl-6 pl-2">
                    <div title="Mark as completed" @click="completedTodos(todo)" :class="{'checked' : todo.completed}" class="unchecked cursor-pointer  h-5 w-5 rounded-full"><img :class="{'icon-checked' : todo.completed}" class="w-3 pt-1 m-auto hidden" src="../assets/images/icon-check.svg" alt=""></div>
                    </div>
                    <div class="w-8/12 -ml-4" :class="{'textChecked' : todo.completed}">{{ todo.text }}</div>
                    <div title="Delete todo from list" class="w-1/12" @click="deleteTodo(index)"><img  class="cursor-pointer w-5 mx-auto remove-icon" src="../assets/images/icon-cross.svg" alt=""></div>
                </li>
            </ul>
            </div>
            <!-- <div class="josefin py-5 px-8 options-area rounded">
                <h1 class="w-4/12 todo-length">{{ todos.length }} items left</h1>
                <div class="w-5/12 mx-auto flex  gap-2 btn-control">
                    <button :class="{'active' : true}" @click="myFilter=[true, false]">All</button >
                    <button :class="{'active' : false }"  @click="myFilter=[false]">Active</button >
                    <button :class="{'active' : true }"  @click="myFilter=[true]">Completed</button>
                </div>
                <div class="w-3/12 text-right"><button class="text-right cleared" @click="clearCompleted">Clear Completed</button></div>
            </div> -->

            <!-- <div class="temp-holder hidden josefin rounded text-white pt-7 -mt-6">
                <div class="flex justify-between">
                    <h1 class="w-6/12 todo-length text-white">{{ todos.length }} items left</h1>
                    <div class="w-5/12 text-right"><button class="text-right" @click="clearCompleted">Clear Completed</button></div>
                </div>
                <div class="w-5/12 mx-auto flex  gap-2">
                    <button @click="myFilter=[true, false]">All</button >
                    <button  @click="myFilter=[false]">Active</button >
                    <button   @click="myFilter=[true]">Completed</button>
                </div>
            </div> -->

        </div>


    </div>
  </div>
</template>



<script>
import NightModeButton from '../components/NightModeButton'

export default {
  name: 'AddTask',
  components: {
      NightModeButton
  },
  
  data() {
      return {
        myTodos: '',
        message:'',
        todos:[],
        completed:'',
        myFilter: [true, false],
        mode:'',

      }
  },
    
     beforeMount() {
        this.todos = JSON.parse(localStorage.getItem('todos')) 
        this.currentmode = localStorage.getItem('todoMode')
        this.mode = this.currentmode
        },
    

  methods: {
      addTodo(e) {
        if(this.myTodos != "") {
        this.todo = ({
            text: this.myTodos,
            completed: false
        })
        this.myTodos = '', e.target.style.border = 'none'
        this.saveToLocalstorage()
        } else {
        e.target.style.border = 'red 2px solid'
        }
   
      },
      

      pushToLocalStorage() { localStorage.setItem('todos', JSON.stringify(this.todos))},

      deleteTodo() { this.deleteFromLocalstorage() },
        
      completedTodos(todo) {
        todo.completed = !todo.completed; this.pushToLocalStorage()
      },

      saveToLocalstorage() {
        this.todos.push(this.todo); this.pushToLocalStorage();
      },

      deleteFromLocalstorage(index) {
        this.todos.splice(index, 1); this.pushToLocalStorage();
      },

      clearCompleted() {
        this.todos = this.todos.filter(todo => !todo.completed); this.pushToLocalStorage();
      },

      nightMode() {
       this.mode === 'dark' ? this.mode = 'light' : this.mode = 'dark'
       localStorage.setItem('todoMode', this.mode)
      },

  },
  
  computed: {
     filteredTodos() {
        let myFilter = this.myFilter;
        return this.todos.filter((todo) => {
            return myFilter.indexOf(todo.completed) >= 1; 
        })
        },

    },
    
}
</script>

<style>
.input-group{
    background: hsla(235, 24%, 19%);
    box-shadow: 1px 28px 17px 9px rgba(13,14,20,0.2);
    padding:20px 12px;
    width: 100%;
    color:rgb(223, 223, 223);
    transition: background 0.5s ease-in-out;
}

.dark .input-group{
    background: hsl(0, 0%, 98%);
    box-shadow: 1px 28px 17px 9px rgba(13,14,20,0.2);
    padding:20px 12px;
    width: 100%;
    color: hsl(237, 14%, 26%);
    transition: background 0.5s ease-in-out;
}
.remove-icon {
    display:none;
}
.list-container{
  box-shadow: 1px 28px 17px 9px rgba(13,14,20,0.5);
}
.dark .list-container{
  box-shadow: 1px 28px 17px 9px rgba(211, 212, 219, 0.5);
}
.list-container:hover .remove-icon{
    display:block;
    animation:spin 0.4s linear ;
}
@keyframes spin { 100% { -webkit-transform: rotate(360deg); transform:rotate(360deg); } }

.unchecked{
    border:1px solid white;
}

.dark .unchecked{
    border:1px solid hsla(235, 24%, 19%);
}

.list-container:hover .unchecked{
    border-top: 1px solid hsl(192, 100%, 67%);
    border-left: 1px solid hsl(192, 100%, 67%);
    border-bottom:1px solid hsl(280, 87%, 65%);
    border-right:1px solid hsl(280, 87%, 65%);
    animation:spin 0.4s linear ;
   
}
.checked {
    background-image: linear-gradient(to right, hsl(192, 100%, 67%) , hsl(280, 87%, 65%));
}
.textChecked{
    text-decoration: line-through;
    opacity: 0.5;
}
.icon-checked{
    display: block;
}
.mylists{
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid rgba(255, 255, 255, 0.301);
    background: hsla(235, 24%, 19%);
    padding:20px 12px;
    transition: background 0.5s ease-in-out;
}

.dark .mylists{
    color:hsla(235, 24%, 19%);
    /* background: hsla(0, 0%, 98%,); */
    background: white;
    border-bottom: 1px solid hsla(240, 9%, 61%, 0.404);
    transition: background 0.5s ease-in-out;
}
.imgHolder{
    background-image: url('../assets/images/bg-desktop-dark.jpg');
    transition: background 0.5s ease-in-out;
}
.dark .imgHolder{
    background-image: url('../assets/images/bg-desktop-light.jpg');
    transition: background 0.5s ease-in-out;
}
.hero{
    background: hsl(235, 21%, 11%);
    transition: background 0.5s ease-in-out;
}
.dark{
    background: hsl(236, 33%, 92%);
    transition: background 0.5s ease-in-out;
}
.my-header{
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding:20px 0;
}
.my-header h1{
    font-family: 'Josefin Sans', sans-serif;
    letter-spacing: 1rem;
    font-weight: 700;
    color:white;
    font-size: 2.5rem;
}
.top-holder{
    height: 430px;
    overflow-y:scroll;
    overflow-x:hidden;
    
}
.top-holder::-webkit-scrollbar {
    width: 0.1em;
  }

.top-holder::-webkit-scrollbar-track {
    box-shadow: inset 0 0 2px hsl(192, 100%, 67%)
  }

.top-holder::-webkit-scrollbar-thumb {
    background-image: linear-gradient(to right, hsl(192, 100%, 67%) , hsl(280, 87%, 65%));
    border-radius: 4px;
  }
.options-area{
    color:white;
    background: rgb(31, 33, 56);
    display: flex;
    justify-content: space-between;
    align-items: center;
    transition: background 0.5s ease-in-out;
   
}
.dark .options-area{
    color:hsl(235, 21%, 11%);
    background: rgb(244, 249, 255);
    transition: background 0.5s ease-in-out;
}

.todo-length {
    grid-column: 1 /span 4;
}
.control-btn {
    grid-column: 2 /span 4;
}

.cleared {
    grid-column: 1 /span 4;
}









.top-holder-x{
    height: 540px;
}
.active{
    color:blue;
}
.actived{
    color:rgb(197, 9, 65);
}

</style>