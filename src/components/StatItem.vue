<template>
    <div class="stat" :class="statType" >
        <span>{{ title }}</span>
        <span class="stat-value">{{ value }}</span>
        <span class="stat-unit">{{ unit }}</span>
    </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    name: 'StatItem',
    props: {
        value : {
            type: Number,
        },
        unit: {
            type: String,
        },
        title: {
            type: String,
        },
        statType : {
            type: String,
        }
    },
    setup(){
        // const {value, unit} = toRefs(props)

        const addClass = (e: Event) => {
            console.log('trying...')
            if(e){
                
                console.log('adding class', e)
            }
        }


        return {
            // value,
            // unit,
            addClass,
        }

    }
})
</script>
<style scoped>
.stat{
    margin: 1rem 2rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.stat-value{
    color: var(--white);
    font-size: 3rem;
}

.stat-unit{
    color: var(--dark);
    font-size: 1rem;
    font-weight: 600;
}


@media screen and (min-width: 425px) {
    .stat-value{
        font-size: 5rem;
    }
}
</style>