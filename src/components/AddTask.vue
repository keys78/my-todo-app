<template>
  <div class="hero h-screen" :class="mode">
    <div class="imgHolder h-64 w-full"> </div>

   
    <div class="xl:w-5/12 lg:w-6/12 md:w-7/12 sm:w-9/12 w-11/12 mx-auto -mt-60 ">
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
            <div v-if="todos.length" class="text-white top-holder">
                <transition-group name="list" tag="p">
                    <ul v-for="(todo, index) in filteredTodos" :key="todo" class="list-container">
                        <li class="mylists rounded ">
                            <div class="w-1/12 sm:pl-6 pl-2">
                            <div title="Mark as completed" @click="completedTodos(todo)" :class="{'checked' : todo.completed}" class="unchecked cursor-pointer  h-5 w-5 rounded-full"><img :class="{'icon-checked' : todo.completed}" class="w-3 pt-1 m-auto hidden" src="../assets/images/icon-check.svg" alt=""></div>
                            </div>
                            <div  class="w-8/12 -ml-4">
                                <div :class="{'textChecked' : todo.completed}">{{ todo.text }}</div>
                                <div class="myDate">{{ todo.date }}</div>
                            </div>
                            <div title="Delete todo from list" class="w-1/12" @click="deleteTodo(index)"><img  class="cursor-pointer w-5 mx-auto remove-icon" src="../assets/images/icon-cross.svg" alt=""></div>
                        </li>
                    </ul>
                </transition-group>
            </div>
            <div v-else class="text-gray-100 pt-20 pb-10 v-else-group text-center">
                <img class="mx-auto" src="../assets/images/illustration-empty.svg" alt="sideArrow" />
                <h1 class="font-bold text-xl pt-8">There is nothing here</h1>
                <p class="invoice-inherit bg-transparent">Hey Champ! create a new todo to get started.</p>
            </div>
            <div class="josefin py-5 options-area rounded">
                <h1 class="todo-length">{{ todos.length }} items left</h1>
                <div class="flex md:gap-4 gap-3 btn-control">
                    <button :class="{active : type === ''}" class="focus:outline-none" @click="myFilter('')">All</button >
                    <button :class="{active : type === 'active'}" class="focus:outline-none" @click="myFilter('active')">Active</button >
                    <button :class="{active : type === 'completed'}" class="focus:outline-none" @click="myFilter('completed')">Completed</button>
                </div>
                <div class="cleared-grp text-right"><button class="focus:outline-none" @click="clearCompleted">Clear Completed</button></div>
            </div>

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
        type:'',
        mode:'',

      }
  },
    
     beforeMount() {
          this.newTodos = JSON.parse(localStorage.getItem('todos'))
            if (this.newTodos === null) {
              fetch('./db.json/') 
            .then(res => { return res.json()})
            .then(data => { localStorage['defaultTodo'] = JSON.stringify(data)
           this.todos = JSON.parse(localStorage.getItem('defaultTodo'))
            })
           } else {
               this.todos = this.newTodos
           }
       
        this.currentmode = localStorage.getItem('todoMode')
        this.mode = this.currentmode
        document.title = 'Total todos' + ' ' + '('+ this.todos.length +')'
       
        },
    
     created() {
            setInterval(this.getNow, 1000);
            
            },

  methods: {
      addTodo(e) {
        if(this.myTodos != "") {
        this.todo = ({
            text: this.myTodos,
            completed: false,
            date: this.date
            
        })
       
        this.myTodos = '', e.target.style.border = 'none'
        this.saveToLocalstorage()
        } else {
        e.target.style.border = 'red 2px solid'
        }
   
      },
      

      pushToLocalStorage() { localStorage.setItem('todos', JSON.stringify(this.todos))},

      deleteTodo(index) {
           this.todos.splice(index, 1);
           this.pushToLocalStorage();
           },
        
      completedTodos(todo) {
        todo.completed = !todo.completed; this.pushToLocalStorage()
      },

      saveToLocalstorage() {
        this.todos.unshift(this.todo); this.pushToLocalStorage();
      },

      clearCompleted() {
        this.todos = this.todos.filter(todo => !todo.completed); this.pushToLocalStorage();
      },

      nightMode() {
       this.mode === 'dark' ? this.mode = 'light' : this.mode = 'dark'
       localStorage.setItem('todoMode', this.mode)
      },

      myFilter(type) {
          this.type = type
      },

        getNow: function() {
            const today = new Date();
            const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
            this.date = date
        },

  },
  
  computed: {
     filteredTodos() {
        return this.todos.filter(todo => {
            switch(this.type) {
                case 'active':
                    return !todo.completed;
                case 'completed':
                    return todo.completed;
                default:
                    return true;
            }
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
    padding:16px 12px;
    transition: background 0.5s ease-in-out;
    font-size: 18px;
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
    background: hsla(235, 24%, 19%);
    transition: background 0.5s ease-in-out;
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    align-items: center;
   
}
.dark .options-area{
    color:hsl(235, 21%, 11%);
    background: rgb(234, 243, 255);
    transition: background 0.5s ease-in-out;
}

.btn-control{
  grid-column: span 3;

  margin: 0 auto;
}
.cleared-grp{
    grid-column: span 2;
    padding-right: 25px;
}
.todo-length{
    grid-column: span 1;
    padding-left: 25px;
} 


.myDate{
    font-family: 'Great Vibes', cursive;
    font-size: 16px;
    color:rgb(52, 224, 224);
}
.dark .myDate{
     color:rgb(1, 27, 27);
}

.on2{
    color:white;
}
.dark .on2 {
    color:rgb(4, 4, 44);
}

.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 0.6s;
}
.list-enter, .list-leave-to /* .l */ {
  opacity: 0;
  transform: translateY(30px);
}

.dark .v-else-group{
    color:rgb(4, 4, 44);
}
.top-holder-x{
    height: 560px;
}
.active{
    color:blue;
}

</style>