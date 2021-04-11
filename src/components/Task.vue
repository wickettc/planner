<template>
    <div :class="[task, task.importance]">
        <h3>{{ task.subject }}</h3>
        <!-- <div>{{ task.importance }}</div> -->
        <div>Due at: {{ task.time }}</div>
        <!-- <div></div>     -->
    </div>
</template>

<script>
export default {
    name: 'Task',
    props: {
        task: Object,
    },
};
</script>

<style scoped>
.task {
    display: flex;
    align-items: center;
    justify-content: space-around;
    width: 80%;
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
