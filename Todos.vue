<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todo</h1>
            <input type="text" class="new-todo" placeholder="Ajouter une tache" v-model="newTodo" @keyup.enter="addTodo">
        </header>
        <div class="main">
            <input type="checkbox" v-model="allDone">
            <ul class="todo-list">
                <li class="todo" v-for="todo in filteredTodos" :class="{completed: todo.completed, editing: todo === editing}">
                    <div class="view">
                        <input type="checkbox" v-model="todo.completed" class="toggle">
                      <label @dblclick="editTodo(todo)">{{ todo.name }}</label>  
                    <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
                    <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" @blur="doneEdit" @keyup.esc="cancelEdit" v-focus="todo === editing">
                </li>
            </ul>
        </div>
        <footer class="footer" v-show="todos.length > 0">
            <span class="todo-count"><strong>{{ remaining }}</strong> tâches à faire</span>
            <ul class="filters">
                <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
                <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
                <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
            </ul>
            <button class="clear-completed" v-show="doneTodo" @click.prevent="deleteCompleted">Supprimer les tâches finies</button>      
        </footer>
    </section>
</template>

<script>
import Vue from 'vue'
export default {
    //preparer les data
    data () {
        return {
            
            todos: [],
            newTodo: '',
            filter: 'all',
            editing: null,
            oldTodo: ''
        }
    },
    methods: {
        //pour ajouter l'entree dans la todos
       addTodo () {
           this.todos.push({
               completed: false,
               name: this.newTodo
           })
           // pour vider apres l'ajout
           this.newTodo = ''
       },
       deleteTodo (todo) {
           this.todos = this.todos.filter(i => i !== todo)
       },
       deleteCompleted () {
           this.todos = this.todos.filter(todo => !todo.completed)
       },
       editTodo(todo) {
           this.editing = todo
           this.oldTodo = todo.name
       },
       doneEdit () {
           this.editing = null
       },
       cancelEdit () {
           this.editing.name = this.oldTodo
           this.doneEdit() 
       }
    },
    computed: {
        allDone: {
            get () {
                return this.remaining === 0
            },
            set (value) {
                this.todos.forEach(todo => {
                    todo.completed = value
                })
            }
        },
        remaining () {
           return this.todos.filter(todo => !todo.completed).length 
        },
        doneTodo () {
           return this.todos.filter(todo => todo.completed).length 
        },
        filteredTodos () {
            if (this.filter === 'todo') {
                return this.todos.filter(todo => !todo.completed)
            } else if (this.filter === 'done') {
                return this.todos.filter(todo => todo.completed)
            }
           return this.todos 
        }
    },
    directives: {
        focus (el, value) {
            if (value) {
                Vue.nextTick(() => {
                    el.focus()
                })                
            }
        }
    }
}
</script>
<style src="./todos.css"></style>