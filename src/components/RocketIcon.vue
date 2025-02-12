<template>
    <div class="code-symbol">
        <span class="c1">-</span><span class="c2">}</span><span class="c3">&gt;</span>
    </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    name: 'RocketIcon',
    
})
</script>
<style scoped>

.code-symbol{
    display: flex;
    flex-direction: row;
    justify-content: center;
    transform: scale(0.5);
}

.code-symbol span{
    font-size: 100px;
    font-weight: bold;
    color: var(--light);
    margin:0 -10px;
    user-select: none; /* Standard syntax */
    -webkit-user-select: none; /* Safari */
    -moz-user-select: none; /* Firefox */
    -ms-user-select: none; /* Internet Explorer/Edge */
}

.c1{
    margin-right: -28px !important;
    line-height: 125px;
}

.c3{
    margin-left: -8px !important;
    line-height: 125px;
}

.code-symbol img{
    width: 200px;
    height: 200px;
}

</style>