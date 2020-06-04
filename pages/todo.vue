<template>
    <div class="container">
        <ul>
            <img src="/favicon.ico"/>
            <li v-for="todo in todos" :key="todo.id">
                <input :checked="todo.done" @change="toggle(todo)" type="checkbox">
                <span :class="{ done: todo.done }">{{ todo.text }}</span>
                <button @click="removeTodo(todo)">remove</button>
            </li>
            <li><input @keyup.enter="addTodo" placeholder="What needs to be done?"></li>
        </ul>
    </div>
</template>

<script>
    import { mapMutations } from 'vuex'

    export default {
        computed: {
            todos () {
                return this.$store.state.todos.list
            }
        },
        methods: {
            addTodo (e) {
                this.$store.commit('todos/add', e.target.value)
                e.target.value = ''
            },
            ...mapMutations({
                toggle: 'todos/toggle'
            }),
            removeTodo (todo){
                this.$store.commit('todos/remove', todo)
            }
        },
        async asyncData({app, params}) {
            let res = await apiGetUserInfo(app.$axios, params.id);

            return {
                user: res.data.userData
            }
        }
    }
</script>

<style>
    .done {
        text-decoration: line-through;
    }
    #__layout{
        background-color: #526488;
    }
</style>