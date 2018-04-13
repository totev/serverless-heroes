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
    import random from "lodash/random";
    import replace from "lodash/replace";

    const APIKEY = process.env.VUE_APP_MARVEL_API_KEY

    export default {
        name: "RandomHero",
        data () {
            return {
                hero: {
                    name: 'World known hero',
                    description: 'hero background'
                },
                copyright: "© 2018 MARVEL",
                portrait: ""
            };
        },
        async created () {
            try {
                const heroesFishingRequest = await  axios.get("https://gateway.marvel.com/v1/public/characters", {
                    params: {apikey: APIKEY, limit: 1}
                });

                const randomHeroPos = random(0, heroesFishingRequest.data.data.total)

                const heroesRequest = await  axios.get("https://gateway.marvel.com/v1/public/characters", {
                    params: {apikey: APIKEY, limit: 1, offset:randomHeroPos}
                });

                this.hero = first(heroesRequest.data.data.results)
                const securePortraitUrlPath = replace(this.hero.thumbnail.path, 'http://', 'https://');
                this.portrait = `${securePortraitUrlPath}/portrait_uncanny.${this.hero.thumbnail.extension}`

            } catch (e) {
                console.error(e);
            }


        },
        methods: {},
    }
</script>

<style scoped>
    .bold {
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

    .about {
        padding: 1em;
    }

    .about .description {
        text-align: justify;
    }

    .portrait {
        padding: 1em;
    }

</style>