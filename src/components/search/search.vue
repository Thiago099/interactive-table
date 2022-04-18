<script setup lang="ts">
import { ref, getCurrentInstance, toRefs, nextTick } from 'vue'
const { emit } = getCurrentInstance()
const props = defineProps<{ found: number }>()
const { found } = toRefs(props)
const last_search = ref("")
const search = ref("")
const display_search = ref("")
function updateSearch(event: any){
    emit('input', event.target.value)
    nextTick(()=>{
        if(found.value)
        {
            last_search.value = search.value;
        }
        const current_search = search.value.search(last_search.value);
        if(current_search == 0)
        {
            display_search.value = `<font color="white">${last_search.value}</font><font color="red">${search.value.replace(last_search.value,'')}</font>`;
        }
        else if(current_search >= 0)
        {
            const mid = search.value.split(last_search.value);
            display_search.value = `<font color="red">${mid[0]}</font><font color="white">${last_search.value}</font><font color="red">${mid[1]}</font>`;
        }
        else
        {
            display_search.value = `<font color="red">${search.value}</font>`;
        }
    })
    
}
</script>

<style scoped lang="less">
@import 'search';
</style>
<template>
    <div style="max-width:800px;margin:auto;margin-bottom:50px;margin-top:40px">
    <div style="margin:20px 0 20px 0;">
        <div v-html="display_search" class="search-input-overlay"></div>
        <i aria-hidden="true" class="fa fa-search fa-fw search-icon"></i>
        <input type="text" class="form-control input-sm search-input" v-model="search" @input="updateSearch" placeholder="Digite para buscar"/>
    </div>
    </div>
</template>