<template>
    <div class="landing-container">
        <h3>.rocket</h3>
        <RocketIcon/>
        <!-- <div class="code-symbol">
            <img src="../assets/icon_square.png" alt="code">
        </div> -->
        <div class="details">
            <p>A <span class="strike">useless</span>, <span class="strike">not-so-</span>rocket-fast converter of common data formats. Convert your json file to xml or yaml and more... </p> 
            <p>Tech stack used: 
                <a href="https://vuejs.org/" class="tech-link" target="_blank">Vue3</a>, 
                <a href="https://docs.spring.io/spring-boot/index.html" class="tech-link" target="_blank">Spring Boot</a>, 
                <a href="https://redis.io/
                " class="tech-link" target="_blank">Redis</a>. Deployed on 
                <a class="tech-link" target="_blank" href="https://render.com">Render</a>
            </p>
        </div>

    </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
import RocketIcon from './RocketIcon.vue';

export default defineComponent({
    name: 'LandingComp',
    components: {
        RocketIcon,
    },
    setup(){

    }
})

</script>

<style scoped>

.landing-container{
    min-height: 100vh;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.title{
    display:flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
}

h3{
    font-size: 2.5rem;
    font-weight: bold;
    letter-spacing: 0.5rem;
    color: var(--white);
    margin:1rem;
}

.details{
    margin: 1rem;
    /* border-top: 1px solid var(--dark); */
}

.details p{
    color: var(--dark);
    margin: 0.5rem 2rem;
    font-size: 1.25rem;
    font-weight: 800;
}

.tech-link{
    color:var(--light);
    font-style: normal;
    text-decoration: none;
}

.strike{
    text-decoration: line-through;
}

@media screen and (min-width:1024px) {
    .details{
        margin: 1rem 10rem;
    }
}
</style>