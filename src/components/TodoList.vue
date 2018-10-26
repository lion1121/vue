<template>
    <div>
        <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo"
               @keyup.enter="addTodo">

        <div class="todo-item"
             v-for="(todo, index) in todos"
             :key="todo.id">

            <div class="todo-item-left">
                <!--Visible when todo.ediding = false-->
                <div class="todo-item-label"
                     v-if="!todo.editing"
                     @dblclick="editTodo(todo)">{{todo.title}}
                </div>

                <!--If todo.editing = true  hidden-->
                <input type="text"
                       v-else class="todo-item-edit"
                       v-model="todo.title"
                       @blur="doneEdit(todo)"
                       @keyup.enter="doneEdit(todo)"
                       @keyup.esc="cancelEdit(todo)"
                       v-focus>
            </div>

            <div class="remove-item"
                 @click="removeTodo(index)">
                &times;
            </div>
        </div>

    </div>

</template>

<script>
    export default {
        name: "todo-list",
        data() {
            return {
                newTodo: '',
                idForTodo: 3,
                TitleForTodo: 'Some title',
                CompletedForTodo: false,
                beforeEditCashe: '',
                todos: [
                    {
                        'id': 1,
                        'title': 'Finish vue Screencast',
                        'completed': false,
                        'editing': false
                    },
                    {
                        'id': 2,
                        'title': 'Take over world',
                        'completed': false,
                        'editing': false
                    },
                ]
            }
        },
        // Custom directive (From documentation)
        directives: {
            focus: {
                inserted: function (el) {
                    el.focus();
                }
            }
        },
        methods: {
            addTodo() {
                if (this.newTodo.trim().length === 0) {
                    return;
                }
                this.todos.push({
                    id: this.idForTodo,
                    title: this.newTodo,
                    completed: this.CompletedForTodo,
                });
                this.newTodo = '';
                this.idForTodo++
            },
            removeTodo(index) {
                this.todos.splice(index, 1);
                // console.log(index);
            },
            editTodo(todo) {
                this.beforeEditCashe = todo.title;
                todo.editing = true;
            },
            doneEdit(todo) {
                if(todo.title.trim() === ''){
                    todo.title = this.beforeEditCashe;
                }
                todo.editing = false;
            },
            cancelEdit(todo){
                todo.title = this.beforeEditCashe;
                todo.editing = false;
            }
        }
    }
</script>

<style lang="scss">
    .todo-input {
        width: 100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;
        &:focus {
            outline: 0;
        }
    }

    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .remove-item {
        cursor: pointer;
        margin-left: 14px;
        &:hover {
            color: #000;
        }
    }

    .todo-item-left { // later
        display: flex;
        align-items: center;
    }

    .todo-item-label {
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }

    .todo-item-edit {
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc; //override defaults
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        &:focus {
            outline: none;
        }
    }

    .completed {
        text-decoration: line-through;
        color: grey;
    }

    .extra-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;
    }

    button {
        font-size: 14px;
        background-color: white;
        appearance: none;
        &:hover {
            background: lightgreen;
        }
        &:focus {
            outline: none;
        }
    }

    .active {
        background: lightgreen;
    }

    // CSS Transitions
    .fade-enter-active, .fade-leave-active {
        transition: opacity .2s;
    }

    .fade-enter, .fade-leave-to {
        opacity: 0;
    }
</style>