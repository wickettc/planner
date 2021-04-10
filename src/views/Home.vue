<template>
    <div class="home">
        <ToDo />
        <AddToDo />
        <Calendar />
    </div>
</template>

<script>
import Calendar from '../components/Calendar';
import ToDo from '../components/ToDo';
import AddToDo from '../components/AddToDo';

export default {
    name: 'Home',
    components: {
        Calendar,
        ToDo,
        AddToDo,
    },
    data() {
        return {
            // showCalendar: true,
        };
    },
};
</script>

<style scoped>
.home {
    display: flex;
    width: 100%;
    justify-content: space-evenly;
}
</style>
