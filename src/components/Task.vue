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
        <div v-if="showNote" :class="['task-note', task.importance]">
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
    color: black;
    border: 1px solid gray;
    background: #30f2f2;
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
    width: 35%;
    overflow-wrap: break-word;
}

.task-time {
    font-size: 1.2rem;
    font-weight: 600;
}

.task-note {
    overflow-wrap: break-word;
    border-top: none;
    padding: 5px 10px;
}

.very-important {
    border-left: 15px solid red;
}

.important {
    border-left: 15px solid orange;
}

.not-important {
    border-left: 15px solid yellow;
}

button {
    background: #77f6f6;
    border: 1px solid white;
    transition: all 0.3s ease-in-out;
    border-radius: 5%;
}

button:hover {
    background: #30f2f2;
    border: 1px solid black;
    transform: scale(1.05);
}
</style>
