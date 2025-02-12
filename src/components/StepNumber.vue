<template>
    <div class="step-number-container">
        <div class="number">{ {{number}}.} </div>
        <div class="title"> <slot></slot></div>
    </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    name: 'StepNumber',
    props: {
        number: Number,
    },
    setup(){

    }
})
</script>
<style scoped>
.step-number-container{
    display: flex;
    flex-direction: row;
    justify-content: center;
    margin: 3rem 0 1rem;
    flex-wrap: wrap;
}

.number, .title{
    font-size: 1.25rem;
    font-weight: 800;
    color: var(--white);
}

.number{
    margin-right: 0.5rem;
}
</style>