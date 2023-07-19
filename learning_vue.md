## The most basic "template looks like this"
### <script>
    export default{
        return{
            //some value
        }
    }
    </script>
    <template>
        //whatever you want to put here
    </template>
## To add dynamicly changing text:
### <script>
    export default{
        //im declaring some reactive states here
        data(){
            return{
                message: 'This is my text'
            }
        }
    }
    </script>
    //now adding the 'formatting' part
    <template>
    //im actually gonna reverse my whole message
    <h1>{{message.split('').reverse().join('')}}</h1>
    </template>
## Mustaches "{}" are only used for text interpolation; to bind attributes to a dynamic value we use "v-bind", ex. "<div v-bind:id="someID"></div>"; you can also just use the shortened version "<div :id="someID"></div>"; to use it something else other than ID, you just switch it up, like so:
#### //this is the JS part

    <script>
        export default{
            data(){
                return{
                    titleClass='title'
                }
            }
        }
    </script>

    //here is the html part 

    <template>
    //because we named the title's class "title", we now can refer to it as so
    <h1 :Class="title">Make me pretty(pink)!</h1>

    //here starts the "css" part
    <style>
    .title{
        color:pink;
    }
    </style>
## Event listeners
## We can listen to DOM by using "<button v-on:click="increment">{{count}}</button>" or the shortened version of it "<button @click="increment">{{count}}</button>". you can me it properly work by addding methods option, like so:
### //JS
    <script>
        export default{
            data(){
                return{
                    //we will count up from -5, becuase that's fun
                    count: -5
                }
            },
            //now we use methods to add some functionality
            methods:{
                increment(){
                    //we're refrencing the count instance by using "this.<name>"
                    this.count++
            }
        }
        }
        </script>
        <template>
            <button @click="increment">It's now: {{count}}</button>
        </template>
# Form bindings
## if we use both v-bind and v-on, we can create two-way binding on input elements, ex. "<inut :value="text" @input="onInput">" and "methods: { onInput(e){ this.text = e.target.value}}". You can for example make a text field to input stuff!
## But you can always just simplify it to: <input v-model="text">. v-model works also on checkboxes, radio buttons & select dropdowns
### //js
    <script>
        export default{
            data(){
                return{
                    text: ''
                }
            },
            methods:{
                onInput(a){
                    this.text= a.target.value
                }
            }
        }
    </script>
    <template>
        <input v-model="text" placeholder="Type here">
        //here is what youre typing:
        <p>{{text}}</p>
    </template>
# Conditional rendering
## We can use ifs! "<h1 v-if="awesome">That's amazing, i love ifs!</h1>" as well as v-else or v-else-if "<h1 v-if="awesome">That's amazing, i love ifs!</h1> <h1 v-else> I dont really enjoy else if and else :c</h1>". we will also implement toggle to switch between them
### 
    <script>
        export default{
            data(){
                return{
                    love: true
                }
            },
            methods:{
                toggle(){
                    this.love != this.love
                }
            }
        }
    </script>

    <template>
    <button @click="toggle">Toggle</button>
    <h1 v-if="love">Love ifs!</h1>
    <h1 v-else>I dont enjoy else :c</h1>
    </template>
# List rendering
## Using for! We can list elements based on a source array, like so: "<ul><li v-for="todo in todos" :key="todo.id"> {{todo.text}}</li></ul>". To update the list we have two options:
- ## 1. Call mutating methods on the source array "this.todos.push(newTodo)"
- ## 2. Replace the array with a new one "this.todos = this.todos.filter(/* ... */)"
## And as per usual, some example, careful, it's quite complex:
###
    <script>
        let id = 0
        export default{
            data(){
                return{
                    newTodo='',
                    todos: [
                        {id: id++, text: 'Basic todo 1'},
                        {id: id++, text: 'Basic todo 2'},
                        {id: id++, text: 'Basic todo 3'}
                    ]
                }
            },
            methods:{
                addTodo(){
                    this.todos.push({id: id++, text: this.newTodo})
                    this.newTodo=''
                }
                removeTodo(){
                    this.todos = this.todos.filter((a) => a!==todo)
                }

            }
        }
    </script>
    //tworzymy formularz do wpisywania
    <template>
        <form @submit.prevent="addTodo">
        <input v-model="newTodo">
        <button>Add a new Todo </button>
        </form>
        <ul>
        <li v-for="todo in todos" :key="todo.id">
        {{todo.text}}
        <button @click="removeTodo(todo)">x</button>
        </li>
        </ul>
    </template>

# Computed property
## First we just add a toggle functionality with a done property, as so: "<li v-for="todo in todos"> <input type="checkbox" v-model="todo.done"> </li>
## We can also add a checkbox in order to be able to hide completed todos, but not neccessarily delete them yet and also filter them, using computed property. "export default{ //... computed:{ filteredTodos(){ //return filtered todos based on this.hideCompleted}}}" and "<li v-for="todo in todos"> <li v-for="todo in filteredTodos">"

### 
    <script>
let id = 0

export default {
  data() {
    return {
      newTodo: '',
      hideCompleted: false,
      todos: [
        { id: id++, text: 'Learn HTML', done: true },
        { id: id++, text: 'Learn JavaScript', done: true },
        { id: id++, text: 'Learn Vue', done: false }
      ]
    }
  },
  computed: {
    filteredTodos() {
      return this.hideCompleted
        ? this.todos.filter((t) => !t.done)
        : this.todos
    }
  },
  methods: {
    addTodo() {
      this.todos.push({ id: id++, text: this.newTodo, done: false })
      this.newTodo = ''
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
    }
  }
}
    </script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>
</template>

<style>
.done {
  text-decoration: line-through;
}
</style>

# Lifecycle and template refs
## We can request a remplate ref, ex. reference to an element in the temaplate "<p ref="pElementRef">hello</p>" it will be exposed on "this.$refs" as "this.refs.pElementRef", but only if the component is mounted. To run code after mount "export default{ mounted(){ //component now mounted}}"

### <script>
export default {
  mounted(){
  this.$refs.pElementRef.textContent = "Wow! It works!"
  }
}
</script>

<template>
  <p ref="pElementRef">hello</p>
</template>

# Watchers
## If we need to log a number that chenges and we wanna do it from the console we use watchers "export default {
  data() {
    return {
      count: 0
    }
  },
  watch: {
    count(newCount) {
      // yes, console.log() is a side effect
      console.log(`new count is: ${newCount}`)
    }
  }
}"
## We use the watcher to to change the count property, if the count changes, it receives the new value from the callback function.
### 
<script>
    export default{
        data(){
            return{
                todoId: 1,
                todoData: null
            }
        },
        methods:{
            async fetchData(){
                this.todoData = null
                const res = await fetch(`https://jsonplaceholder.typicode.com/todos/${this.todoId}`)
                this.todoData = await res.json()
            }
        },
        mounted(){
            this.fetchData()
        },
        watch(){
            todoId(){
                this.todoData()
            }
        }
    }
</script>
<template>
    <p>Todo id: {{todoID}}</p>
    <button @click="todoId++">Fetch next todo </button>
    <p v-if="!todoData">Loading...</p>
    <pre v-else>{{todoData}} </pre>
</template>

# Components
## To use a child component, that's nested inside another one, we first import it "import ChildComp fro './ChildComp.vue' export default{ components:{ ChildComp}}" and then we can use it "<ChildComp />" and use it like so (if we of course have the child component made)
### 
<script>
    import ChildComp from './ChildComp.vue'
    export default{
        components:{
            ChildComp
        }
    }
</script>
<template>
//this will call our child component and execute it
<ChildComp />
</template>

# Props
## If you want your child component to accept something from the parent, use props. First we declare it in child component "export default{ props: {msg: String}}". After that we can use "this" in child component's template. To pass the prop to the child componentin the form of an attribute, use "v-bind" syntax: "<ChildComp :msg="Hello">"
### //This is the parent:
<script>
    import ChildComp from './ChildComp.vue'
    export default{
        components:{
            ChildComp
        },
        data:{
            return{
                greetings: "Hi, it's me, the parent"
            }
            }
    }
</script>
<template>
    <Childcomp :msg="greetings"/>
</template>

### //This is the child
### <script>
export default {
  props: {
    msg: String
  }
}
</script>

<template>
  <h2>{{ msg || 'No props passed yet' }}</h2>
</template>

# Emits
## A child can also emit events to the parent, like so:
## "export default{
    //declare emitted events
    emits: ['response'],
    created(){
        //emit with an argument
        this.$emit('response','hello from child')
    }
}"
## to listen to the emit use:
## "<ChildComp @response="(msg) => childMsg =msg" />"
## And the whole thing looks like so:
### The parent:
### <script>
import ChildComp from './ChildComp.vue'

export default {
  components: {
    ChildComp
  },
  data() {
    return {
      childMsg: 'No child msg yet'
    }
  }
}
</script>

<template>
  <ChildComp @response="(msg) => childMsg=msg"/>
  <p>{{ childMsg }}</p>
</template>

### While the child look like this:
### <script>
export default {
  emits: ['response'],
  created() {
    this.$emit('response', 'hello from child')
  }
}
</script>

<template>
  <h2>Child component</h2>
</template>

# Slots
## The parent can also pass down the component via slots:
## "<ChildComp>
This is some slot content
</ChildComp> "

## To render content in the child component, we use "<slot/>" in the child component
## The content inside will be treated as fallback, so it will be displayed if the parent didn't pass down any content "<slot>Fallback content </slot>"
### This is an example of that, from parent:
### <script>
    import ChildComp from './ChildComp.vue'
    export default{
        components:{
            ChildComp
        },
        data(){
            return{
                msg: 'This is from parent'
            }
        }
    }
    </script>
    <template>
    <ChildComp>{{msg}}. Hope you see this. </ChildComp>
    </template>

### And the child:
### <template>
<slot>This is fallback content, in case parent doesn't text you</slot>
</template>