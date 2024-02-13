<script>
    import axios, { Axios } from '../node_modules/axios/index';
    import AddTaskHeader from './components/AddTaskHeader.vue';

    export default {
        name: "App",

        components: {
            AddTaskHeader
        },
        data() {
            return {
                valueInput: '',
                needDoList: [],
                editingTask: '',
            };
        },
        mounted() {
            axios.get(`https://65c4b2f0dae2304e92e324ec.mockapi.io/todo/tasks/`)
                .then(res => {
                    this.needDoList = res.data;
                    console.log(res.data);
                })
                .catch(error => console.error('Error:', error));
        },
        methods: {
            addTask(task) {
                const currentTask = {
                    name: task,
                    idLocal: Math.random() * 10,
                };
                axios.post(`https://65c4b2f0dae2304e92e324ec.mockapi.io/todo/tasks/`, currentTask)
                    .then(res => {
                        console.log(`Successfully posted`, res.data.name);
                        currentTask.id = res.data.id;
                    })
                    .catch(error => console.error('Error:', error));
                this.needDoList.push(currentTask);
                this.valueInput = '';
            },
            handleDelete(taskToRemove) {
                axios.delete(`https://65c4b2f0dae2304e92e324ec.mockapi.io/todo/tasks/${taskToRemove.id}`)
                    .then(res => console.log(`Successfully deleted ${res.data.name}`))
                    .catch(error => console.error('Error:', error));
                const index = this.needDoList.indexOf(taskToRemove);
                this.needDoList.splice(index, 1);
            },
            editTask(task) {
                console.log(task);
                this.editingTask = task;
            },
            endEditing(task) {
                axios.put(`https://65c4b2f0dae2304e92e324ec.mockapi.io/todo/tasks/${task.id}`, task)
                    .then(res => console.log(`Successfully edited ${res.data.name}`))
                    .catch(error => console.error('Error:', error));
                this.editingTask = null;
            },
        },
    }
</script>

<template>
    <div id="app">
        <add-task-header @add-task="addTask" />
        <div class="container">
            <h2>
                <span>Tasks To Do:</span>
            </h2>
            <ul v-for="task in needDoList"
                :key="task.idTask"
                class="task-list">
                <li>
                    <span v-if="task !== editingTask" @click="editTask(task)">{{ task.name }}</span>
                    <input type="text" v-model="task.name" v-if="task === editingTask" @keydown.enter="endEditing(task)">
                    <div>
                        <input type="checkbox" />
                        <button @click="handleDelete(task)"
                                class="btn-remove">
                            <span class="span-text">
                                x
                            </span>
                        </button>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>

<style src="./App.css">
</style>
