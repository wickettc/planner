<template>
    <div class="task-container">
        <div :class="['task', task.importance]">
            <h3>{{ task.subject }}</h3>
            <!-- <div>{{ task.importance }}</div> -->
            <div class="task-time">Time: {{ task.time }}</div>
            <button @click="showNote = !showNote">
                {{ !showNote ? 'Show Note' : 'Close Note' }}
            </button>
        </div>
        <div v-if="showNote" class="task-note">
            {{ task.notes }}
            <div v-if="!task.notes">Sorry, no notes</div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Task',
    props: {
        task: Object,
    },
    data() {
        return {
            showNote: false,
        };
    },
};
</script>

<style scoped>
.task-container {
    width: 95%;
    margin-bottom: 5px;
}

.task {
    display: flex;
    align-items: center;
    justify-content: space-around;
    width: 100%;
    box-sizing: border-box;
    font-size: 1.3rem;
}

.task h3 {
    width: 45%;
    word-break: break-all;
}

.task-time {
    font-size: 1.2rem;
    font-weight: 600;
}

.task-note {
    word-break: break-all;
    border: 1px solid black;
    border-top: none;
}

.very-important {
    background-color: red;
}

.important {
    background-color: orange;
}

.not-important {
    background-color: yellow;
}
</style>
