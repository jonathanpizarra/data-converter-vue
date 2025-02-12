<template>

    <div class="stats-container">
        <StatItem :value="fileCountValue" :unit="fileCountUnit" :title="``" :stat-type="`stat-total`"/>
        <StatItem :value="sizeCountValue" :unit="sizeCountUnit" :title="``" :stat-type="`stat-total`"/>
    </div>

</template>

<script lang="ts">
import { defineComponent } from 'vue';
import StatItem from './StatItem.vue';

export default defineComponent({
    name: 'StatComp',
    components: {
        StatItem,
    },
    setup(){
        let fileCountValue: number = 42
        let fileCountUnit: string = 'files converted'
        let sizeCountValue: number = 123
        let sizeCountUnit: string = 'bytes processed'

        return{
            fileCountValue,
            fileCountUnit,
            sizeCountValue,
            sizeCountUnit,
        }
    }
})
</script>

<style scoped>

.stats-container{
    border-top: 1px solid var(--dark);
    border-bottom: 1px solid var(--dark);
    margin:1rem;
    padding:1rem 0;
    display: flex;
    flex-direction: row;
    justify-content: center;
    flex-wrap: wrap;
}

</style>