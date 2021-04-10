<template>
    <div>
        <div class="todo-header">
            <h2>Today</h2>
            <button>Add</button>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            today: undefined,
        };
    },
};
</script>

<style scoped>
.todo-header {
    display: flex;
    width: 100%;
    justify-content: space-evenly;
}
</style>
