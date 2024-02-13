<template>
    <div class="header">
        <div class="container">
            <div class="logo">ToDo List</div>
            <div class="form">
                <input type="text"
                       v-model="valueInput"
                       @keydown.enter="addTask"
                       maxlength="25">
                <button class="btn" @click="addTask">add</button>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                valueInput: '',
                needDoList: []
            }
        },
        methods: {
            addTask() {
                this.needDoList.push(this.valueInput);
                this.$emit('add-task', this.valueInput);
                this.valueInput = '';
            }
        }
    }

</script>
<style>
    button {
        margin-left: 2px;
    }
</style>