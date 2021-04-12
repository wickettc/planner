<template>
    <div class="app">
        <Calendar
            @selected-date="setSelectedDate"
            :class="['v-calendar', showCalendar ? 'show-calendar' : '']"
        />
        <AddTask
            @add-task="addTask"
            :class="['v-addtask', showAddTask ? 'show-add-task' : '']"
        />
        <DisplayTasks
            :selectedDate="selectedDate"
            :todaysTasks="todaysTasks"
            @cal-clicked="handleCalendarClick"
            @addtask-clicked="handleAddTaskClick"
            @ask-delete-task="handlePopup"
            class="v-display-tasks"
        />
        <Popup
            @yes-delete-task="handleDeleteYes"
            @no-delete-task="handleDeleteNo"
            :task="taskToDelete"
            v-if="showPopup"
        />
    </div>
</template>

<script>
import Calendar from './components/Calendar';
import DisplayTasks from './components/DisplayTasks';
import AddTask from './components/AddTask';
import Popup from './components/Popup';

export default {
    components: {
        Calendar,
        DisplayTasks,
        AddTask,
        Popup,
    },
    methods: {
        addTask(task) {
            // finds todo obj if selected date already exists
            let foundToDo = this.getTodaysTasks();
            // adds onto that date obj if is there
            if (foundToDo) {
                foundToDo = foundToDo.tasks.push(task);
                this.tasks = [...this.tasks, foundToDo];
                // creates new date obj if nothing is found
            } else {
                this.tasks = [
                    ...this.tasks,
                    { date: this.selectedDate, tasks: [task] },
                ];
            }
        },
        deleteTask(deleteTask) {
            let foundTodays = this.getTodaysTasks();
            // gets index of task to remove
            let updatedTodays = foundTodays.tasks.findIndex(
                (task) => task.id == deleteTask
            );
            // finds todays tasks and then remove index found above
            this.tasks
                .find((toDoObj) => toDoObj.date === this.selectedDate)
                .tasks.splice(updatedTodays, 1);
        },
        setSelectedDate(selectedDate) {
            this.selectedDate = selectedDate;
        },
        getTodaysTasks() {
            return this.tasks.find(
                (toDoObj) => toDoObj.date === this.selectedDate
            );
        },
        handleCalendarClick() {
            this.showCalendar = !this.showCalendar;
        },
        handleAddTaskClick() {
            this.showAddTask = !this.showAddTask;
        },
        handlePopup(task) {
            this.taskToDelete = task;
            this.showPopup = true;
        },
        handleDeleteYes(task) {
            this.showPopup = false;
            this.deleteTask(task.id);
            this.taskToDelete = {};
        },
        handleDeleteNo() {
            this.showPopup = false;
            this.taskToDelete = {};
        },
    },
    data() {
        return {
            tasks: [], //{date: String, tasks: Array}
            todaysTasks: [],
            taskToDelete: {},
            selectedDate: '',
            showCalendar: false,
            showAddTask: false,
            showPopup: false,
        };
    },
    watch: {
        selectedDate: function() {
            let todays = this.getTodaysTasks();
            // sets todaysTodos if there are any
            if (todays) {
                this.todaysTasks = todays.tasks;
                // clears last day if there are none on switch of selectedDate
            } else {
                this.todaysTasks = [];
            }
        },
        tasks: function() {
            let todays = this.getTodaysTasks();
            // prevents error if last task of day is deleted
            if (todays) {
                this.todaysTasks = todays.tasks;
            } else {
                this.todaysTasks = [];
            }
        },
    },
};
</script>

<style>
html,
body {
    padding: 0;
    margin: 0;
    height: 100vh;
    font-size: 16px;
}
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}

button {
    padding: 6px 10px;
    font-size: 1.2rem;
    cursor: pointer;
}

select {
    cursor: pointer;
}

::-webkit-scrollbar {
    width: 5px;
}

::-webkit-scrollbar-track {
    background: rgba(110, 109, 109, 0.507);
}

::-webkit-scrollbar-thumb {
    background: #f6f677;
}

::-webkit-scrollbar-thumb:hover {
    background: #f2f230;
}

@media only screen and (max-width: 500px) {
    html {
        font-size: 14px !important;
    }
}
</style>

<style scoped>
.app {
    height: 100vh;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    grid-template-areas:
        'displaytasks calendar'
        'displaytasks addtask';
}

.v-display-tasks {
    box-sizing: border-box;
    grid-area: displaytasks;
    border-right: 3px solid white;
    height: 100vh;
    width: 50vw;
    background: #a7f8dd;
}

.v-calendar {
    box-sizing: border-box;
    grid-area: calendar;
    border-bottom: 3px solid white;
    width: 50vw;
    background: #c3f8c3;
}

.v-addtask {
    box-sizing: border-box;
    grid-area: addtask;
    width: 50vw;
    background: #ddf8a7;
}

@media only screen and (max-width: 767px) {
    .app {
        height: 100%;
        display: flex;
        flex-direction: column;
    }

    .v-display-tasks {
        border-bottom: 3px solid white;
        border-right: none;
        width: 100%;
    }

    .v-calendar {
        display: none;
        border-bottom: 3px solid white;
        width: 100%;
    }

    .v-addtask {
        display: none;
        border-bottom: 3px solid white;
        width: 100%;
    }

    .show-add-task {
        display: flex;
    }

    .show-calendar {
        display: block;
    }
}
</style>
