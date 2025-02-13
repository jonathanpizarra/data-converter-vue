<template>

    <div class="stats-container">
        <StatItem :value="fileCountValue" :unit="fileCountUnit" :title="``" :stat-type="`stat-total`"/>
        <StatItem :value="sizeCountValue" :unit="sizeCountUnit" :title="``" :stat-type="`stat-total`"/>
    </div>

</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue';
import StatItem from './StatItem.vue';
import axios from 'axios';

export default defineComponent({
    name: 'StatComp',
    components: {
        StatItem,
    },
    setup(_, {expose}){
        const fileCountValue = ref(0)
        let fileCountUnit: string = 'files converted'
        const sizeCountValue = ref(0)
        let sizeCountUnit: string = 'bytes processed'

        const setStats = async () =>{
            console.log('setting stats...')
            const resp = await axios.get( process.env.VUE_APP_API_URL + 'stats' )

            if(resp.status == 200){
                console.log('resp.data', resp.data)
                fileCountValue.value = resp.data.fileCount
                sizeCountValue.value = resp.data.byteCount
            }
        }

        onMounted(setStats)

        expose({setStats})

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