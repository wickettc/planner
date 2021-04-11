<template>
    <!-- <div id="nav">
        <router-link to="/">Home</router-link> |
        <router-link to="/about">About</router-link>
    </div> -->
    <router-view />
</template>

<style>
html,
body {
    padding: 0;
    margin: 0;
    height: 100vh;
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
