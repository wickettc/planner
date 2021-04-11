<template>
    <div class="addtodo-container">
        <form @submit.prevent="onSubmit">
            <h3>Add To-do</h3>
            <input
                v-model="subject"
                type="text"
                name="subject"
                placeholder="What is the to-do?"
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
                v-model="notes"
                rows="6"
                name="notes"
                placeholder="Any notes?"
            ></textarea>
            <button>Add Task</button>
        </form>
    </div>
</template>

<script>
import uniqid from 'uniqid';

export default {
    name: 'AddToDo',
    data() {
        return {
            times: [],
            subject: '',
            importance: '',
            time: '',
            notes: '',
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
                this.subErr = 'Please Add a To-do';
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
                const newTodo = {
                    id: uniqid(),
                    subject: this.subject,
                    importance: this.importance,
                    time: this.time,
                    notes: this.notes,
                };
                this.subject = '';
                this.importance = '';
                this.time = '';
                this.notes = '';
                this.subErr = '';
                this.impErr = '';
                this.timeErr = '';
                this.$emit('add-todo', newTodo);
            }
        },
    },
};
</script>

<style scoped>
h3 {
    margin: 10px 0;
}
.addtodo-container {
    display: flex;
    justify-content: center;
    align-items: center;
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
}

select {
    text-align-last: center;
    -moz-text-align-last: center;
}
</style>
