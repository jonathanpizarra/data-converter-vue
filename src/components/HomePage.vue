<template>
    <div class="converter">
        <LandingComp />
        <StatComp ref="statComp"/>

        <div class="upload-container">
            <RocketIcon/>
            <div class="step-container">
                <StepNumber :number="1">Select your source format</StepNumber>

                <div class="format-container">
                    <div class="format-container-item" v-for="(format,index) in formats" :key="format">
                        <p v-if="index > 0 && isNotMobile">|</p>
                        <p :class="generateClass(format)" @click="selectFormat(format)">.{{ format }}</p>
                    </div>
                </div>
                <div class="line"></div>
            </div>

            <div class="step-container">
                <StepNumber :number="2">Select your target format</StepNumber>

                <div class="target-selector">
                    <p class="target-selector-text">From <span class="chosen-format">.{{ chosenFormat }}</span> to </p>
                    <select name="selectTarget" id="selectTarget" v-model="targetFormat">
                        <option v-for="f in formatTargets[chosenFormat]" :value="f" :key="f">.{{ f }}</option>
                    </select>
                </div>
                <div class="line"></div>
            </div>

            <FileUpload @updateStats="updateStatsParent" :sourceFormat="chosenFormat" :targetFormat="targetFormat" ref="fileUploadRef" :clearParentInputs="clearParentInputs"/>

        </div>

        <FooterComp/>
    </div>
</template>

<script lang="ts">
import { computed, defineComponent, onMounted, onUpdated, ref, useTemplateRef } from 'vue';
import FileUpload from './FileUpload.vue';
import LandingComp from './LandingComp.vue';
import StatComp from './StatComp.vue';
import FooterComp from './FooterComp.vue';
import StepNumber from './StepNumber.vue';
import RocketIcon from './RocketIcon.vue';

interface Formats<T = string[]> {
    [key: string] : T
}

// type FileUploadInstance = InstanceType<typeof FileUpload>;

export default defineComponent({
    name : 'HomePage',
    components: {
        FileUpload,
        LandingComp,
        StatComp,
        FooterComp,
        StepNumber,
        RocketIcon,
    },
    setup(){
        const formats = ['json', 'csv', 'xml', 'xlsx']
        const formatTargets: Formats = {
            json: ['xml', 'yaml'],
            csv: ['json', 'yaml', 'xml'],
            xml: ['json', 'yaml'],
            xlsx: ['json', 'yaml', 'xml'],
            yaml: ['']
        }

        const chosenFormat = ref('json')
        const targetFormat = ref('xml')
        const suffix = ref('s')
        const conversionType = ref('')
        const windowWidth = ref(window.innerWidth)
        const fileUploadRef = useTemplateRef<typeof FileUpload>('fileUploadRef')
        const statComp = useTemplateRef<typeof StatComp>('statComp')

        const selectFormat = (format: string): void => {

            let el = document.querySelector(`.format-${format}`)
            if(el){
                document.querySelector(`.format-${chosenFormat.value}`)?.classList.remove('selected-source')
                el.classList.add('selected-source')
                chosenFormat.value = format
                targetFormat.value = ""
                fileUploadRef?.value?.clearInputs?.()

            }
            
        }

        const generateClass = (format: string): string => {
            return `format format-${format}`
        }

        const clearParentInputs = (): void => {
            targetFormat.value = ''
        }

        const updateStatsParent = (): void => {
            // call child method
            statComp.value?.setStats?.()
        }

        const isNotMobile = computed(() => windowWidth.value > 768)

        // Debug: Check the ref on updated (useful if there are reactive dependencies)
        onUpdated(() => {
            console.log('onUpdated - Child component ref:', statComp.value);
        });

        onMounted(()=>{
            window.addEventListener('resize', ()=> windowWidth.value = window.innerWidth)
            console.log('onMounted - Child component ref:', statComp.value);
            let el = document.querySelector(`.format-${chosenFormat.value}`)
            if(el){
                document.querySelector('selected-source')?.classList.remove('selected-source')
                el.classList.add('selected-source')
            }
        })

        return {
            formats,
            formatTargets,
            chosenFormat,
            targetFormat,
            suffix,
            conversionType,
            selectFormat,
            generateClass,
            fileUploadRef,
            clearParentInputs,
            updateStatsParent,
            isNotMobile,
        }
    }
    
})
</script>

<style scoped>

.converter{
    height: 100vh;
    min-height: 100vh;
}

.format-container{
    margin: 0 1rem;
}

.format-container, .format-container-item{
    display: flex;
    flex-direction: row;
    justify-content: center;
    flex-wrap: wrap;
}

.format-container-item p {
    margin: 1.5rem;
    font-size: 1.5rem;
    color: var(--light);
}

.format{
    color: var(--light);
}

.format:hover{
    cursor: pointer;

}

.selected-source, select{
    color: var(--black) !important;
    font-weight: 600;
}

.target-selector{
    font-size: 1rem;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
}

.target-selector-text{
    display: flex;
    flex-direction: row;
    color: var(--light);
}

.chosen-format{
    display: block;
    text-decoration: none;
    font-style: normal;
    color:var(--black);
    width: 5rem;
    margin: 0 0.15rem;
    font-weight: 500;

}

select {
    font-size: 1rem;
    margin-left: 0.5rem;
    border:none;
    /* border-bottom: 1px solid var(--white); */
    background-color: var(--mid);
    width: 7rem;
    color:var(--black);
    
}

option{
    font-size: 1rem;
    color:var(--black);
}

@media screen and (min-width: 425px) {
    .target-selector, select{
        font-size: 1.5rem;
    }
    select{
        margin-left: 1rem;
    }

    option{
        font-size: 1.3rem;
    }

    .chosen-format{
        margin: 0 0.5rem;
    }

    .upload-container{
        margin-top: 6rem;
    }
}



</style>