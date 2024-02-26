<template>
    <div id="app">
        <add-task-header @add-task="addTask" />
        <div class="container">
            <h2>
                <span>Tasks To Do:</span>
            </h2>
            <add-list :need-do-list="needDoList" @delete-task="handleDelete" @update-data="updateData" />
        </div>

    </div>
</template>

<script>
    import axios from '../node_modules/axios/index';
    import AddTaskHeader from './components/AddTaskHeader.vue';
    import AddList from './components/AddList.vue';

    export default {
        name: "App",

        components: {
            AddTaskHeader,
            AddList,
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
            updateData(data) {
                this.needDoList = data;
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
