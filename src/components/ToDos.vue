<template>
    <div>
        <div class="todos-header">
            <h2>Tasks for {{ selectedDate }}</h2>
        </div>
        <div class="todos-container" v-for="task in todaysTodos" :key="task.id">
            <Task :task="task" />
        </div>
        <div v-if="todaysTodos.length === 0">No tasks to display</div>
        <Footer />
    </div>
</template>

<script>
import Task from './Task';
import Footer from './Footer';

export default {
    name: 'ToDos',
    props: {
        todaysTodos: Array,
        selectedDate: String,
    },
    components: {
        Task,
        Footer,
    },
};
</script>

<style scoped>
h2 {
    font-size: 2rem;
}
.todos-header {
    display: flex;
    width: 100%;
    justify-content: space-evenly;
}

.todos-container {
    display: flex;
    justify-content: center;
    width: 100%;
    overflow-y: auto;
}
</style>
