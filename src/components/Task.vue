<template>
    <div class="task-container">
        <div :class="['task', task.importance]">
            <h3>{{ task.subject }}</h3>
            <div class="task-time">Time: {{ task.time }}</div>
            <div @click="showDetails = !showDetails" class="task-details">
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                >
                    <path
                        d="M12 2c5.514 0 10 4.486 10 10s-4.486 10-10 10-10-4.486-10-10 4.486-10 10-10zm0-2c-6.627 0-12 5.373-12 12s5.373 12 12 12 12-5.373 12-12-5.373-12-12-12zm-.001 5.75c.69 0 1.251.56 1.251 1.25s-.561 1.25-1.251 1.25-1.249-.56-1.249-1.25.559-1.25 1.249-1.25zm2.001 12.25h-4v-1c.484-.179 1-.201 1-.735v-4.467c0-.534-.516-.618-1-.797v-1h3v6.265c0 .535.517.558 1 .735v.999z"
                    />
                </svg>
            </div>
            <div @click="$emit('ask-delete-task', task)" class="task-delete">
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="30"
                    height="30"
                    viewBox="0 0 24 24"
                >
                    <path
                        d="M12 0c-4.992 0-10 1.242-10 3.144 0 .406 3.556 18.488 3.633 18.887 1.135 1.313 3.735 1.969 6.334 1.969 2.601 0 5.199-.656 6.335-1.969.081-.404 3.698-18.468 3.698-18.882 0-2.473-7.338-3.149-10-3.149zm0 1.86c4.211 0 7.625.746 7.625 1.667 0 .92-3.414 1.667-7.625 1.667s-7.625-.746-7.625-1.667 3.414-1.667 7.625-1.667zm4.469 19.139c-.777.532-2.418 1.001-4.502 1.001-2.081 0-3.721-.467-4.498-.998l-.004-.021c-1.552-7.913-2.414-12.369-2.894-14.882 3.55 1.456 11.304 1.455 14.849-.002-.868 4.471-2.434 12.322-2.951 14.902zm-7.872-7.418l-.492-.323 1.824-.008.78 1.667-.506-.32c-.723 1.146-1.027 1.764-.796 2.481-1.823-1.798-1.622-2.182-.81-3.497zm.622-1.304l.781-1.418c.195-.38 1.251-.075 1.688.899l-.797 1.445-1.672-.926zm2.673 5.175h-1.729c-.427.013-.672-1.061-.031-1.915h1.761v1.915zm.058-4.886l.524-.289c-.652-1.188-1.044-1.753-1.781-1.898 2.451-.729 2.593-.41 3.445.981l.521-.275-.79 1.654-1.919-.173zm3.059.005l.911 1.474c.236.355-.546 1.129-1.607 1.035l-.928-1.501 1.624-1.008zm-1.549 4.846l-.004.583-1.028-1.616 1.054-1.47-.006.6c1.354.011 2.037-.055 2.524-.63-.565 2.5-.942 2.533-2.54 2.533z"
                    />
                </svg>
            </div>
        </div>
        <div v-if="showDetails" :class="['task-detail', task.importance]">
            {{ task.details }}
            <div v-if="!task.details">Sorry, no details</div>
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
            showDetails: false,
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
    font-size: 1.2rem;
}

.task h3 {
    width: 30%;
    overflow-wrap: break-word;
}

.task-time {
    font-weight: 600;
    width: 30%;
}

.task-delete,
.task-details {
    cursor: pointer;
    display: flex;
    transition: all 0.3s ease-in-out;
}

.task-delete:hover,
.task-details:hover {
    transform: scale(1.1);
}

.task-detail {
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
    border: 1px solid black;
    transition: all 0.3s ease-in-out;
    border-radius: 5%;
}

button:hover {
    background: #30f2f2;
    transform: scale(1.05);
}
</style>
