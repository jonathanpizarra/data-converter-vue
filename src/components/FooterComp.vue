<template>
    <div class="padding"></div>
    <RocketIcon/>
    <div class="padding"></div>
    <div class="footer-container">
        <p>&copy; Jonathan Pizarra 2025</p>
        <p>|</p>
        <p><a href="https://github.com/jonathanpizarra">Github</a></p>
        
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import RocketIcon from './RocketIcon.vue';

export default defineComponent({
    name: 'FooterComp',
    components:{
        RocketIcon,
    },
    props: {
        title: String,
    }
})

</script>

<style scoped>
.footer-container{
    display: flex;
    flex-direction: row;
    justify-content: center;
    font-size: 0.75rem;
}

.footer-container p {
    margin:0.5rem;
    color: var(--light);
}

.footer-container p a{
    color: var(--light);
    text-decoration: none;
}

.padding{
    height: 5rem;
    width: 100%;
}

@media screen and (min-width:425px) {
    .footer-container p {
        margin:1rem;
    }
}
</style>