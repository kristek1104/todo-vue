<template>
    <div>
        <ul class="task-list">
            <li v-for="task in needDoList"
                :key="task.idTask">
                <span v-if="task !== editingTask" @click="editTask(task)">{{ task.name }}</span>
                <input type="text" v-if="task === editingTask" v-model="task.name" @keydown.enter="endEditing(task)">
                <div>
                    <input type="checkbox" v-if="task.checked === false" @click="checkedTask(task)" />
                    <input type="checkbox" v-else="task.checked === true" checked @click="checkedTask(task)" />
                    <button class="btn-remove"
                            @click="deleteTask(task)">
                        <span class="span-text">
                            x
                        </span>
                    </button>
                </div>
            </li>
        </ul>
    </div>
</template>
<script>
    import axios from '../../node_modules/axios/index';
    export default {
        data() {
            return {
                valueInput: '',
                editingTask: '',
            };
        },
        props: {
            needDoList: {
                type: Array,
                requered: true,
            },
        },
        mounted() {
            axios.get(`https://65c4b2f0dae2304e92e324ec.mockapi.io/todo/tasks/`)
                .then(res => {
                    this.$emit('update-data', res.data);
                })
                .catch(error => console.error('Error:', error));
        },
        methods: {
            deleteTask(taskToRemove) {
                this.$emit('delete-task', taskToRemove);

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