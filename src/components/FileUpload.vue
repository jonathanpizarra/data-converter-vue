<template>
    <form class="file-upload-container" @submit.prevent="uploadFile" >

        <div class="step-container">
            <StepNumber :number="3">Select your file</StepNumber>
            <div class="file-input-box">
                <label class="custom-file-input">
                    <span>Choose your file</span>
                    <input type="file" max="1" @change="onFileChange" id="fileInput">
                </label>
                <span class="filename">{{ filename }}</span>
            </div>
            <div class="line"></div>

        </div>
        <div class="step-container">
            <StepNumber :number="4">Select available customizations</StepNumber>

            <div class="options-container">
                <div class="option suffix-input-container" v-if="sourceFormat == 'json' && targetFormat == 'xml'">
                    <span>Suffix for list items: </span>
                    <input type="text" name="suffix" v-model="suffix" maxlength="10">
                    <div class="sample-result-box">
                        <pre>//Suffix for lists</pre>
                        <pre>&lt;user{{ suffix }}&gt;</pre>
                        <pre>  &lt;user/&gt;</pre>
                        <pre>  &lt;user/&gt;...</pre>
                        <pre>&lt;user{{ suffix }}&gt;...</pre>
                    </div>
                </div>
                <div class="option conversion-input-container" v-else-if="sourceFormat == 'xlsx' && targetFormat == 'xml'">
                    <div class="conversion-input-box">
                        <span>Conversion type format</span>
                        <label for="conversionType">
                            <input type="radio" name="conversionType" value="PROPER" v-model="conversionType">
                            <span>Default</span>
                        </label>
                        <label for="conversionType">
                            <input type="radio" name="conversionType" value="LIST" v-model="conversionType">
                            <span>List style</span>
                        </label>
                    </div>
                    <div class="conversion-input-sample">
                        <div class="conversion-input-sample-proper" v-if="conversionType == 'PROPER'">
                            <pre>&lt;Sheet&gt;</pre>
                            <pre>    &lt;Row&gt;</pre>
                            <pre>        &lt;Cell Column="0"&gt;UserName&lt;/Cell&gt;</pre>
                            <pre>        &lt;Cell Column="1"&gt;Details&lt;/Cell&gt;</pre>
                            <pre>    &lt;/Row&gt;...</pre>
                            <pre>&lt;/Sheet&gt;</pre>
                        </div>
                        <div class="conversion-input-sample-list" v-else-if="conversionType == 'LIST'">
                            <pre>&lt;Records&gt;</pre>
                            <pre>    &lt;Record&gt;</pre>
                            <pre>        &lt;UserName&gt;John Doe&lt;/Cell&gt;</pre>
                            <pre>        &lt;Details&gt;Just a chill guy&lt;/Cell&gt;</pre>
                            <pre>    &lt;/Record&gt;...</pre>
                            <pre>&lt;/Records&gt;</pre>
                        </div>
                    </div>
                </div>
                <div class="option no-options-container" v-else>
                    <span>No available options for selected formats.</span>
                </div>
            </div>
            <div class="line"></div>
            
        </div>

        <div class="step-container">
            <StepNumber :number="5">Verify and Convert</StepNumber>
            <div class="button-container">
                <button @click="clearInputs">Clear</button>
                <button type="submit">Convert</button>
            </div>
        </div>
    </form>
    <LoaderComp :is-visible="isLoading"/>
    <ErrorModal v-if="isError" :closeErrorModal="closeErrorModal">{{ errMsg }}</ErrorModal>

</template>

<script lang="ts">
import { defineComponent, ref, toRefs} from 'vue';
import axios from 'axios';
import StepNumber from './StepNumber.vue';
import LoaderComp from './Modals/LoaderComp.vue';
import ErrorModal from './Modals/ErrorModal.vue';

export default defineComponent({
    name: "FileUpload",
    components: {
        StepNumber,
        LoaderComp,
        ErrorModal,
    },
    props: {
        sourceFormat: {
            type: String,
        },
        targetFormat: {
            type: String,
        },
        clearParentInputs:{
            type: Function,
        }
    },
    emits:['updateStats'],
    setup(props, {emit}){
        const file = ref()
        const suffix = ref('s')
        const filename = ref('No file selected.')
        const conversionType = ref('PROPER')
        const isLoading = ref(false)
        const isError = ref(false)
        const errMsg = ref('')
        
        const {sourceFormat, targetFormat, clearParentInputs} = toRefs(props)
        const mimeTypes = {
            json: 'application/json',
            xml: 'text/xml',
            csv: 'text/csv',
            xlsx: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'
        } as { [key: string] : string }

        const uploadFile = async () =>{
            if(!file.value || !targetFormat.value || targetFormat.value == ''){
                errMsg.value = 'Please fill out all fields'
                isError.value = true
                return
            }

            let formData = new FormData()
            formData.append(sourceFormat.value + 'File', file.value)

            if(sourceFormat.value == 'json' && targetFormat.value == 'xml'){
                if(!suffix.value){
                     errMsg.value = 'Please fill out all fields'
                     isError.value = true
                     return
                }
                formData.append('suffix', suffix.value)
            }

            if(sourceFormat.value == 'xlsx' && targetFormat.value == 'xml'){
                if(!conversionType.value){
                    errMsg.value = 'Please fill out all fields'
                    isError.value = true
                    return
                }
                formData.append('conversionType', conversionType.value)
            }
            isLoading.value = true

            try{
                const resp = await axios.post(
                    process.env.VUE_APP_API_URL  + sourceFormat.value + '/to-' + targetFormat.value, 
                    formData,
                    {
                        responseType: 'blob',
                        headers: {
                            'Content-Type': 'multipart/form-data'
                        }
                    }
                )

                if(resp.status == 200){
                    const url = window.URL.createObjectURL(resp.data)
                    const link = document.createElement('a')
                    link.href = url
                    link.setAttribute('download', `converted_file.${targetFormat.value}`)
                    document.body.appendChild(link)
                    link.click()
                    isLoading.value = false
                    clearInputs()
                    emit('updateStats')
                }else{
                    isLoading.value = false
                    errMsg.value = 'Error uploading your file. Please try again'
                    isError.value = true
                }
            }catch(err){
                isLoading.value = false
                errMsg.value = 'Error converting your file. Please try again'
                isError.value = true
            }

        }

        const onFileChange = (e: Event)=>{
            const inputFile = e.target as HTMLInputElement
            console.log('event: ', e)
            console.log('inputFile', e.target)
            if(inputFile.files && inputFile.files[0] && sourceFormat.value){
                console.log('file : ', inputFile.files[0])
                console.log('source: ', sourceFormat.value)
                console.log('target', targetFormat.value)
                if(inputFile.files[0].type != mimeTypes[sourceFormat.value]){
                    errMsg.value = 'Invalid file for selected source format.'
                    isError.value = true
                    return
                }
                
                file.value = inputFile.files[0]
                filename.value = inputFile.files[0].name
            }
                
        }

        const clearInputs = (): void => {
            file.value = undefined
            filename.value = 'No file selected.'
            suffix.value = 's'
            conversionType.value = 'PROPER'
            clearParentInputs.value?.()
        }

        const closeErrorModal = (): void =>{
            isError.value = false
            errMsg.value = ''
        }


        return {
            file,
            suffix,
            filename,
            conversionType,
            isLoading,
            isError,
            errMsg,
            closeErrorModal,
            uploadFile,
            clearInputs,
            onFileChange,
            
        } 
    }

})
</script>

<style scoped>
form{
    margin: 0;
    padding: 0;

}

.file-input-box{
    display: flex;
    flex-direction: column;
    align-items: center;
}

.filename{
    font-size: 1rem;
    height: 1.5rem;
    color:var(--dark);
    margin:1rem 0;
    text-decoration: underline;
}

.custom-file-input {
    display: inline-block;
    position: relative;
    overflow: hidden;
    cursor: pointer;
}

.custom-file-input input[type="file"] {
    position: absolute;
    top: 0;
    right: 0;
    margin: 0;
    padding: 0;
    cursor: pointer;
    opacity: 0;
}

.custom-file-input span {
    display: inline-block;
    padding: 10px 20px;
    background-color: var(--light);
    color: var(--dark);
    border-radius: 4px;
}

.suffix-input-container{
    margin:1rem;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    font-size: 1rem;
    flex-wrap: wrap;

}

.suffix-input-container span{
    color: var(--light);
}

.suffix-input-container input{
    margin: 1rem;
    color: var(--dark);
    background-color: var(--light);
    border: 1px solid var(--mid);
    border-radius: 4px;
    height: 2rem;
    padding: 0 0.5rem;
    font-size: 1.2rem;
}

button{
    padding: 0.6rem 2rem;
    background-color: var(--light);
    border-radius: 4px;
    border:1px solid var(--mid);
    font-size: 1rem;
    color:var(--dark);
    margin:1rem;
}

button:hover{
    cursor: pointer;
    color:var(--black);
}

.option{
    margin:1rem;
}

.sample-result-box, .conversion-input-sample div{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    border: 1px solid var(--dark);
    border-radius: 4px;
    padding: 1rem;
    margin: 1rem;
}

.sample-result-box pre, .conversion-input-sample div pre{
    margin: 0;
    color: var(--dark);
    font-size: 0.9rem;
}

.conversion-input-container{
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
}

.conversion-input-box{
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.conversion-input-box span{
    font-size: 1rem;
    color: var(--black);
}

.conversion-input-box span, .conversion-input-box label{
    margin:0.25rem 0;
    display: flex;
    flex-direction: row;
    color:var(--light);
}

.button-container{
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 2rem;
}

.button-container button{
    margin: 0 1rem;
}

.no-options-container span{
    color: var(--dark);
}

</style>