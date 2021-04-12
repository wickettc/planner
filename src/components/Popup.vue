<template>
    <div class="popup">
        <h2>Are you sure you want to delete the "{{ task.subject }}" task?</h2>
        <div class="btns">
            <button @click="$emit('yes-delete-task', task)">Yes</button>
            <button @click="$emit('no-delete-task')">No</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Popup',
    props: {
        task: Object,
    },
};
</script>

<style scoped>
.popup {
    z-index: 10000;
    position: fixed;
    left: 50%;
    top: 50%;
    max-width: 80%;
    transform: translate(-50%, -50%);
    border: 3px solid black;
    background-color: gray;
    padding: 3%;
    display: flex;
    flex-direction: column;
    text-align: center;
    color: black;
}

.popup h2 {
    overflow-wrap: break-word;
}

.btns {
    display: flex;
    justify-content: space-evenly;
    flex-direction: column;
    margin: 15px;
}

button {
    border: 1px solid black;
    transition: all 0.3s ease-in-out;
}

button:hover {
    transform: scale(1.1);
}

button:first-child {
    margin-bottom: 5px;
    color: red;
}
@media only screen and (max-width: 767px) {
    .btns {
        padding: 6%;
    }
}
</style>
