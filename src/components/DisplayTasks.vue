<template>
    <div>
        <div class="displaytasks-header">
            <div @click="addTaskBtnClicked" class="show-addtask-btn">
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="30"
                    height="30"
                    viewBox="0 0 24 24"
                >
                    <path
                        d="M9 19h-4v-2h4v2zm2.946-4.036l3.107 3.105-4.112.931 1.005-4.036zm12.054-5.839l-7.898 7.996-3.202-3.202 7.898-7.995 3.202 3.201zm-6 8.92v3.955h-16v-18h4l2.102 2h3.898l2-2h4v1.911l2-2.024v-1.887h-3c-1.229 0-2.18-1.084-3-2h-8c-.82.916-1.771 2-3 2h-3v22h20v-7.98l-2 2.025zm-8-16.045c.552 0 1 .448 1 1s-.448 1-1 1-1-.448-1-1 .448-1 1-1z"
                    />
                </svg>
            </div>
            <h2>Tasks for {{ selectedDate }}</h2>
            <div @click="calBtnClicked" class="show-calendar-btn">
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="30"
                    height="30"
                    viewBox="0 0 24 24"
                >
                    <path
                        d="M17 1c0-.552-.447-1-1-1s-1 .448-1 1v2c0 .552.447 1 1 1s1-.448 1-1v-2zm-12 2c0 .552-.447 1-1 1s-1-.448-1-1v-2c0-.552.447-1 1-1s1 .448 1 1v2zm13 5v10h-16v-10h16zm2-6h-2v1c0 1.103-.897 2-2 2s-2-.897-2-2v-1h-8v1c0 1.103-.897 2-2 2s-2-.897-2-2v-1h-2v18h20v-18zm4 3v19h-22v-2h20v-17h2zm-17 7h-2v-2h2v2zm4 0h-2v-2h2v2zm4 0h-2v-2h2v2zm-8 4h-2v-2h2v2zm4 0h-2v-2h2v2zm4 0h-2v-2h2v2z"
                    />
                </svg>
            </div>
        </div>
        <div class="all-displaytasks-container">
            <div
                class="displaytasks-container"
                v-for="task in todaysTasks"
                :key="task.id"
            >
                <Task
                    @ask-delete-task="$emit('ask-delete-task', task)"
                    :task="task"
                />
            </div>
            <div class="no-task-div" v-if="todaysTasks.length === 0">
                No tasks to display yet, let's either add a task or select a
                diffent day to add a task too!
            </div>
        </div>
        <Footer />
    </div>
</template>

<script>
import Task from './Task';
import Footer from './Footer';

export default {
    name: 'DisplayTasks',
    props: {
        todaysTasks: Array,
        selectedDate: String,
    },
    components: {
        Task,
        Footer,
    },
    methods: {
        calBtnClicked() {
            this.$emit('cal-clicked');
        },
        addTaskBtnClicked() {
            this.$emit('addtask-clicked');
        },
    },
    emits: ['ask-delete-task', 'cal-clicked', 'addtask-clicked'],
};
</script>

<style scoped>
h2 {
    font-size: 2rem;
}
.displaytasks-header {
    display: flex;
    width: 100%;
    justify-content: space-evenly;
    align-items: center;
    height: 15%;
}

.all-displaytasks-container {
    overflow-y: auto;
    height: 80%;
}

.displaytasks-container {
    display: flex;
    justify-content: center;
    width: 100%;
}

.no-task-div {
    margin: 0 15%;
    font-weight: 600;
    font-size: 1.15rem;
}

.show-calendar-btn,
.show-addtask-btn {
    display: none;
    padding: 10px 10px;
    border: 2px solid black;
    border-radius: 25%;
    transition: all 0.3s ease-in-out;
}

.show-calendar-btn:hover,
.show-addtask-btn:hover {
    transform: scale(1.05);
}

@media only screen and (max-width: 767px) {
    .show-calendar-btn,
    .show-addtask-btn {
        margin: 5px;
        display: block;
    }
}
</style>
