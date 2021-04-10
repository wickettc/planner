<template>
    <form>
        <h3>Add To-do</h3>
        <input type="text" name="todo" placeholder="What is the to-do?" />
        <select>
            <option v-for="(t, index) in times" :key="index" value="t">
                {{ t }}
            </option>
        </select>
        <select>
            <option value="v-important">Very Important</option>
            <option value="important">Important</option>
            <option value="n-important">Not Important</option>
        </select>
        <button>Add</button>
    </form>
</template>

<script>
export default {
    data() {
        return {
            times: [],
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
};
</script>

<style scoped>
form {
    display: flex;
    flex-direction: column;
}
</style>
