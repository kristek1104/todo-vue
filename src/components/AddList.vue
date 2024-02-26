<template>
    <div @click="handleClick">
        <ul class="task-list">
            <li v-for="task in needDoList"
                :key="task.idTask">
                <span v-if="task !== editingTask" class="taskSpan" @click="editTask(task)">{{ task.name }}</span>
                <input v-if="task === editingTask" v-model="task.name" type="text" maxlength="25" @keydown.enter="endEditing(task)" v-click-away="onClickAway">
                <div>
                    <input v-if="task.checked === false" type="checkbox" @click="checkedTask(task)" />
                    <input v-else="task.checked === true" checked type="checkbox" @click="checkedTask(task)" />
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
                .catch(error => console.error('Error in getting tasks:', error));
        },
        methods: {
            onClickAway() {
                    axios.put(`https://65c4b2f0dae2304e92e324ec.mockapi.io/todo/tasks/${this.editingTask.id}`, this.editingTask)
                        .catch(error => console.error('Error in editing task:', error));
                    this.editingTask = null;
            },
            deleteTask(taskToRemove) {
                this.$emit('delete-task', taskToRemove);

            },
            editTask(task) {
                this.editingTask = task;
            },
            endEditing(task) {
                axios.put(`https://65c4b2f0dae2304e92e324ec.mockapi.io/todo/tasks/${task.id}`, task)
                    .catch(error => console.error('Error in finish editing task:', error));
                this.editingTask = null;
            },
            checkedTask(task) {
                const checkedTask = {
                    checked: task.checked === true ? task.checked = false : task.checked = true,
                };
                axios.put(`https://65c4b2f0dae2304e92e324ec.mockapi.io/todo/tasks/${task.id}`, checkedTask)
                    .catch(error => console.error('Error in checking task:', error));

            },
        },
    }
</script>
<style>
    .taskSpan {
        width: 300px;
        text-align: left
    }
</style>

