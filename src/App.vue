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
                    <input type="text" v-if="task === editingTask" v-model="task.name" @keydown.enter="endEditing(task)">
                    <div>
                        <input type="checkbox" v-if="task.checked === false" @click="checkedTask(task)" />
                        <input type="checkbox" v-else="task.checked === true" checked @click="checkedTask(task)" />
                        <button class="btn-remove"
                                @click="handleDelete(task)">
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

<script>
    import axios from '../node_modules/axios/index';
    import AddTaskHeader from './components/AddTaskHeader.vue';

    export default {
        name: "App",

        components: {
            AddTaskHeader,
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
                })
                .catch(error => console.error('Error:', error));
        },
        methods: {
            addTask(task) {
                const currentTask = {
                    name: task,
                    idLocal: Math.random() * 10,
                    checked: false,
                };
                axios.post(`https://65c4b2f0dae2304e92e324ec.mockapi.io/todo/tasks/`, currentTask)
                    .then(res => {
                        currentTask.id = res.data.id;
                    })
                    .catch(error => console.error('Error:', error));
                this.needDoList.push(currentTask);
                this.valueInput = '';
            },
            handleDelete(taskToRemove) {
                axios.delete(`https://65c4b2f0dae2304e92e324ec.mockapi.io/todo/tasks/${taskToRemove.id}`)
                    .catch(error => console.error('Error:', error));
                const index = this.needDoList.indexOf(taskToRemove);
                this.needDoList.splice(index, 1);
            },
            editTask(task) {
                this.editingTask = task;
            },
            endEditing(task) {
                axios.put(`https://65c4b2f0dae2304e92e324ec.mockapi.io/todo/tasks/${task.id}`, task)
                    .catch(error => console.error('Error:', error));
                this.editingTask = null;
            },
            checkedTask(task) {
                const checkedTask = {
                    checked: task.checked === true ? task.checked = false : task.checked = true,
                };
                axios.put(`https://65c4b2f0dae2304e92e324ec.mockapi.io/todo/tasks/${task.id}`, checkedTask)
                    .catch(error => console.error('Error:', error));

            },
        },
    }
</script>

<style src="./App.css">
</style>
