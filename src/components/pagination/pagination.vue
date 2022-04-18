<script setup lang="ts">
import { ref, computed, getCurrentInstance } from 'vue'

const {pages} = defineProps<{ pages: number, page:number }>()
const { emit } = getCurrentInstance()
const page_display = 15
const page = ref(0)

const display_pages = computed(() => {
    const result = []
    let start;
    let end;
    if(page_display >= pages - 4)
    {
        start = 1;
        end = pages;
    }
    else
    {
        let leftpad = Math.ceil(page_display/2)
        let rightpad = Math.ceil(page_display/2)
        if(page.value - leftpad < 0)
        {
            rightpad -= page.value - leftpad 
            leftpad = 1
        }
        else
        {
            leftpad = page.value - leftpad + 1
        }
        if(page.value + rightpad > pages)
        {
            leftpad -= page.value + rightpad - pages
            rightpad = pages
        }
        else
        {
            rightpad = page.value + rightpad
        }
        start = leftpad
        end = rightpad
        
        if(start <= 1)
        {
            end +=1
        }
        if(start <= 2)
        {
            end +=1
        }
        if(end >= pages)
        {
            start -= 1;
        }
        if(end >= pages-1)
        {
            start -= 1;
        }
    }
    for(let i = start; i <= end; i++){
        result.push(i)
    }
    return result
})

function setPage(value: number){
    page.value = Number(value);
    emit('page', page.value)
}

function nextPage(){
    page.value++;
    emit('page', page.value)
}

function prevPage(){
    page.value--;
    emit('page', page.value)
}

</script>
<style scoped lang="less">
@import 'pagination.less';
</style>

<template>
    <div class="col-12 d-flex justify-content-center">
        <nav aria-label="Page navigation example">
            <ul class="pagination">
                <li class="page-item">
                    <a class="page-link" href="#" @click.prevent="prevPage()" :class="{'disabled':page == 1}">
                        <span aria-hidden="true">
                            <i class="fa fa-angle-left" aria-hidden="true"></i>
                        </span>
                    </a>
                </li>
                <li class="page-item" :key="index" v-if="display_pages[0] != 1">
                    <a href="#" @click.prevent="setPage(1)" class="page-link" :class="{'page-link-selected': page == 1}">
                        1
                    </a>
                </li>
                <li class="page-item">
                    <a class="page-link disabled no-select"  v-if="display_pages[0] != 1 && display_pages[0] != 2">
                        <span aria-hidden="true">
                            ...
                        </span>
                    </a>
                </li>
                <li class="page-item" v-for="index in display_pages" :key="index">
                    <a href="#" @click.prevent="setPage(index)" class="page-link" :class="{'page-link-selected': page == index}">
                        {{ index }}
                    </a>
                </li>
                <li class="page-item" v-if="display_pages[display_pages.length-1] != pages && display_pages[display_pages.length-1] != pages-1">
                    <a class="page-link disabled no-select">
                        <span aria-hidden="true">
                            ...
                        </span>
                    </a>
                </li>
                <li class="page-item" v-if="display_pages[display_pages.length-1] != pages">
                    <a href="#" @click.prevent="setPage(pages)" class="page-link" :class="{'page-link-selected': page == pages}">
                        {{ pages }}
                    </a>
                </li>
                <li class="page-item">
                    <a class="page-link" href="#" @click.prevent="nextPage()" :class="{'disabled':page == pages}">
                        <span aria-hidden="true">
                            <i class="fa fa-angle-right" aria-hidden="true"></i>
                        </span>
                    </a>
                </li>
            </ul>
        </nav>
    </div>
</template>