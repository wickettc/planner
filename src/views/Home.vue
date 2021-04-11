<template>
    <div class="home">
        <ToDos
            :selectedDate="selectedDate"
            :todaysTodos="todaysTodos"
            class="v-todo"
        />
        <Calendar @selected-date="setSelectedDate" class="v-calendar" />
        <AddToDo @add-todo="addTodo" class="v-addtodo" />
    </div>
</template>

<script>
import Calendar from '../components/Calendar';
import ToDos from '../components/ToDos';
import AddToDo from '../components/AddToDo';

export default {
    name: 'Home',
    components: {
        Calendar,
        ToDos,
        AddToDo,
    },
    methods: {
        addTodo(todo) {
            // finds todo obj if selected date already exists
            let foundToDo = this.getTodaysTodos();
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
        getTodaysTodos() {
            return this.todos.find(
                (toDoObj) => toDoObj.date === this.selectedDate
            );
        },
    },
    data() {
        return {
            todos: [], //{date: String, tasks: Array}
            todaysTodos: [],
            selectedDate: '',
        };
    },
    watch: {
        selectedDate: function() {
            let todays = this.getTodaysTodos();
            // sets todaysTodos if there are any
            if (todays) {
                this.todaysTodos = todays.tasks;
                // clears last day if there are none on switch of selectedDate
            } else {
                this.todaysTodos = [];
            }
        },
        todos: function() {
            let todays = this.getTodaysTodos();
            this.todaysTodos = todays.tasks;
        },
    },
};
</script>

<style scoped>
.home {
    height: 100vh;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    grid-template-areas:
        'todo calendar'
        'todo addtodo';
}

.v-todo {
    box-sizing: border-box;
    grid-area: todo;
    border-right: 3px solid black;
    height: 100vh;
}

.v-calendar {
    box-sizing: border-box;
    grid-area: calendar;
    border-bottom: 3px solid black;
}

.v-addtodo {
    box-sizing: border-box;
    grid-area: addtodo;
    width: 100%;
}
</style>
