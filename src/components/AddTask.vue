<template>
    <div class="add-task-container">
        <form @submit.prevent="onSubmit" autocomplete="off">
            <h3>Add Task</h3>
            <input
                v-model="subject"
                type="text"
                name="subject"
                placeholder="What is the task?"
            />
            <div class="error sub-err">{{ subErr }}</div>
            <select v-model="importance" name="importance">
                <option selected disabled value=""
                    >Please Select Level of Importance</option
                >
                <option value="very-important">Very Important</option>
                <option value="important">Important</option>
                <option value="not-important">Not Important</option>
            </select>
            <div class="error imp-err">{{ impErr }}</div>
            <select v-model="time" name="time">
                <option selected disabled value=""
                    >Please Select the Time</option
                >
                <option
                    v-for="(time, index) in times"
                    :key="index"
                    :value="time"
                >
                    {{ time }}
                </option>
            </select>
            <div class="error time-err">{{ timeErr }}</div>
            <textarea
                v-model="details"
                rows="6"
                name="details"
                placeholder="Any details?"
            ></textarea>
            <button>Add Task</button>
        </form>
    </div>
</template>

<script>
import uniqid from 'uniqid';

export default {
    name: 'AddTask',
    data() {
        return {
            times: [],
            subject: '',
            importance: '',
            time: '',
            details: '',
            subErr: '',
            impErr: '',
            timeErr: '',
        };
    },
    created() {
        for (let i = 0; i < 2; i++) {
            for (let j = 1; j < 13; j++) {
                i === 0
                    ? this.times.push(`${j} AM`)
                    : this.times.push(`${j} PM`);
            }
        }
    },
    methods: {
        onSubmit() {
            if (!this.subject) {
                this.subErr = 'Please Add a Task';
            } else {
                this.subErr = '';
            }
            if (this.importance === '') {
                this.impErr = 'Make Sure to Select Level of Importance';
            } else {
                this.impErr = '';
            }
            if (this.time === '') {
                this.timeErr = 'Make Sure to Select the Time';
            } else {
                this.timeErr = '';
            }
            if (!this.subErr && !this.impErr && !this.timeErr) {
                const newTask = {
                    id: uniqid(),
                    subject: this.subject,
                    importance: this.importance,
                    time: this.time,
                    details: this.details,
                };
                this.subject = '';
                this.importance = '';
                this.time = '';
                this.details = '';
                this.subErr = '';
                this.impErr = '';
                this.timeErr = '';
                this.$emit('add-task', newTask);
            }
        },
    },
};
</script>

<style scoped>
h3 {
    margin: 10px 0;
}
.add-task-container {
    display: flex;
    justify-content: center;
    align-items: center;
    padding-bottom: 5px;
}

.error {
    color: red;
}

form {
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    width: 80%;
    height: 100%;
}

input,
textarea,
select {
    padding: 8px 10px;
    text-align: center;
    background: #cdf57f;
    border: 1px solid white;
    font-size: 1.1rem;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    border-radius: 5%;
}

select {
    text-align-last: center;
    -moz-text-align-last: center;
}

select option[value='very-important'] {
    background: red;
}

select option[value='important'] {
    background: orange;
}

select option[value='not-important'] {
    background: yellow;
}

button {
    background: #cdf57f;
    border: 1px solid white;
    border-radius: 5%;
    transition: all 0.3s ease-in-out;
}

button:hover {
    background: #d6f695;
    transform: scale(1.05);
    border: 1px solid black;
}

@media only screen and (max-width: 767px) {
    .add-task-container {
        padding: 15px 0;
    }
}
</style>
