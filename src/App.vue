<template>
    <div id="app">
        <RandomHero></RandomHero>
    </div>
</template>

<script>
    import RandomHero from './components/RandomHero.vue'

    export default {
        name: 'app',
        components: {
            RandomHero
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
