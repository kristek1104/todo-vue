<template>
    <ul v-for="task in needDoList"
        :key="task.id"
        class="task-list">
        <li>
            <span>{{ task.name }}</span>
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
</template>

<script>
    export default {
        data() {
            return {
                valueInput: '',
                needDoList: [],
            };
        },
        methods: {
            handleDelete(taskToRemove) {
                this.needDoList = this.needDoList.filter(t => t !== taskToRemove);
                this.$emit('delete', this.needDoList);

            },
            editTask: function (task) {
                this.editingTask = task;
                this.$emit('edit', this.editingTask);
            },
            endEditing: function () {
                this.editingTask = null;
                this.$emit('endEdit', this.editingTask);
            },
        }
    }
</script>