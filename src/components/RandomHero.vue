<template>
    <div v-if="hero">
        <div class="random-hero">
            <div class="about">
                <div class="bold">{{hero.name}}</div>
                <div class="description">{{hero.description}}</div>
            </div>
            <div class="portrait">
                <img :src="portrait"/>
            </div>
        </div>

        <footer>Copyright api and api-response: {{ copyright }}</footer>
    </div>

</template>

<script>
    import axios from "axios";
    import first from "lodash/first";
    const APIKEY = process.env.VUE_APP_MARVEL_API_KEY

    export default {
        name: "RandomHero",
        data () {
            return {
                hero: undefined,
                copyright: "© 2018 MARVEL",
                portrait: ""
            };
        },
        async created () {
            const heroesRequest = await  axios.get("https://gateway.marvel.com/v1/public/characters/1009351", {
                params: {apikey: APIKEY}
            });
            const remoteHero = heroesRequest.data.data
            this.hero = first(remoteHero.results)
            console.log("baml!", JSON.stringify(this.hero));
            this.portrait = `${this.hero.thumbnail.path}/portrait_uncanny.${this.hero.thumbnail.extension}`

        },
        methods: {},
    }
</script>

<style scoped>
    .bold{
        font-weight: bold;
    }

    footer {
        position: absolute;
        right: 0;
        bottom: 0;
        left: 0;
        padding: 1rem;
        background-color: #efeeea;
        text-align: center;
    }

    .random-hero {
        width: 100%;
        heght: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .about{
        padding: 1em;
    }

    .about .description{
        text-align: justify;
    }

    .portrait{
        padding: 1em;
    }

</style>