<template>
    <div class="loader-container" v-if="isVisible">
        <div class="dots">
            <span class="dot dot1">.</span> 
            <span class="dot dot2">.</span> 
            <span class="dot dot3">.</span>
        </div>
        <RocketIcon/>
    </div>
</template>
<script lang="ts">
import { defineComponent} from 'vue';
import RocketIcon from './RocketIcon.vue';

export default defineComponent({
    name: 'LoaderComp',
    components: {
        RocketIcon,
    },
    props:{
        isVisible: Boolean,
    },
    setup() {
        
    }
})

</script>
<style scoped>
.loader-container{
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    position: fixed;
    top:0;
    left:0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
}

.dots{
    display: flex;
    flex-direction: row;
    width: 9.6rem;
    margin-right: -1rem;
}

.dot{
    margin:0.5rem;
    font-size: 3.5rem;
    color: var(--light);
    padding-bottom: 1.6rem;
}

.dot1{
    opacity: 0;
    animation: animateDot1 1.5s infinite ;
}

.dot2{
    opacity: 0;
    animation: animateDot2 1.5s infinite ;
}

.dot3{
    opacity: 0;
    animation: animateDot3 1.5s infinite ;
}

@keyframes animateDot1 {
    0%{
        opacity: 0;
    }
    50%{
        opacity: 0;
    }
    75%{
        opacity: 1;
    }
    100%{
        opacity: 0;
    }
    
}

@keyframes animateDot2 {
    0%{
        opacity: 0;
    }
    25%{
        opacity:0;
    }
    50%{
        opacity: 1;
    }
    75%{
        opacity: 0;
    }
    100%{
        opacity: 0;
    }
    
}

@keyframes animateDot3 {
    0%{
        opacity: 0;
    }
    25%{
        opacity: 1;
    }
    50%{
        opacity: 0;
    }
    100%{
        opacity: 0;
    }
    
}
</style>