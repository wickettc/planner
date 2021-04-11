<template>
    <div class="app">
        <Calendar
            @selected-date="setSelectedDate"
            :class="['v-calendar', showCalendar ? 'show-calendar' : '']"
        />
        <AddTask
            @add-todo="addTodo"
            :class="['v-addtodo', showAddTask ? 'show-add-task' : '']"
        />
        <DisplayTasks
            :selectedDate="selectedDate"
            :todaysTasks="todaysTasks"
            @cal-clicked="handleCalendarClick"
            @addtask-clicked="handleAddTaskClick"
            class="v-display-tasks"
        />
    </div>
</template>

<script>
import Calendar from './components/Calendar';
import DisplayTasks from './components/DisplayTasks';
import AddTask from './components/AddTask';

export default {
    components: {
        Calendar,
        DisplayTasks,
        AddTask,
    },
    methods: {
        addTodo(todo) {
            // finds todo obj if selected date already exists
            let foundToDo = this.getTodaysTasks();
            // adds onto that date obj if is there
            if (foundToDo) {
                foundToDo = foundToDo.tasks.push(todo);
                this.todos = [...this.todos, foundToDo];
                // creates new date obj if nothing is found
            } else {
                this.todos = [
                    ...this.todos,
                    { date: this.selectedDate, tasks: [todo] },
                ];
            }
        },
        setSelectedDate(selectedDate) {
            this.selectedDate = selectedDate;
        },
        getTodaysTasks() {
            return this.todos.find(
                (toDoObj) => toDoObj.date === this.selectedDate
            );
        },
        handleCalendarClick() {
            this.showCalendar = !this.showCalendar;
        },
        handleAddTaskClick() {
            this.showAddTask = !this.showAddTask;
        },
    },
    data() {
        return {
            todos: [], //{date: String, tasks: Array}
            todaysTasks: [],
            selectedDate: '',
            showCalendar: false,
            showAddTask: false,
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
        todos: function() {
            let todays = this.getTodaysTasks();
            this.todaysTasks = todays.tasks;
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
        'todo calendar'
        'todo addtodo';
}

.v-display-tasks {
    box-sizing: border-box;
    grid-area: todo;
    border-right: 3px solid white;
    height: 100vh;
    background: #95f6d6;
}

.v-calendar {
    box-sizing: border-box;
    grid-area: calendar;
    border-bottom: 3px solid white;
    background: #b7f7b7;
}

.v-addtodo {
    box-sizing: border-box;
    grid-area: addtodo;
    width: 100%;
    background: #d6f695;
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
    }

    .v-calendar {
        display: none;
        border-bottom: 3px solid white;
    }

    .v-addtodo {
        display: none;
        border-bottom: 3px solid white;
    }

    .show-add-task {
        display: flex;
    }

    .show-calendar {
        display: block;
    }
}
</style>
