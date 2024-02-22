<template>
    <div class="header">
        <div class="container">
            <div class="logo">ToDo List</div>
            <div class="form">
                <input v-model="valueInput"
                       type="text"
                       maxlength="25"
                       @keydown.enter="addTask">
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
    .btn {
        margin-left: 2px;
    }

    .logo {
        font-size: 20px
    }
</style>