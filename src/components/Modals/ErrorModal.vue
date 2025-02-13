<template>
    <div class="error-container">
        <div class="error-box">
            <p class="error-msg"><slot></slot></p>
            <button @click="closeButton">OK</button>
        </div>
    </div>
</template>
<script lang="ts">
import { defineComponent, toRefs } from 'vue';

export default defineComponent({
    name: 'ErrorModal',
    props:{
        msg: String,
        closeErrorModal: Function,
    },
    setup(props){
        const {closeErrorModal} = toRefs(props)

        const closeButton = (): void=>{
            closeErrorModal.value?.()
        }

        return{
            closeButton,
        }

    }
})
</script>
<style scoped>
.error-container{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
}

.error-box{
    background-color: var(--light);
    border: 1px solid var(--dark);
    border-radius: 6px;
    padding: 1rem;
    width: 20rem;
}

button{
    font-size: 1rem;
    width: 5rem;
    height: 2rem;
    background-color: var(--white);
    color:var(--dark);
    font-weight: 500;
    border: 1px solid var(--dark);
    border-radius: 4px;

}
button:hover{
    color: var(--black);
}
</style>