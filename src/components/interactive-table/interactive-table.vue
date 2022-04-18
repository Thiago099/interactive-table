<script setup lang="ts">
import pagination from '@/components/pagination/pagination.vue'
import search from '@/components/search/search.vue'
import basicTable from './table-display/table-display.vue'
import { ref, toRefs } from 'vue'
const found = ref(true)
const page = ref(1)
const pages = ref(1)
const per_page = ref(10)
const sort_column = ref("null")
const sort_order = ref(false)
const display = ref([] as any[])

const props = defineProps<{ tableFields: string[], tableData:object[] }>()
const { tableFields, tableData  } = toRefs(props)

const filteredData = ref(tableData.value)

function updatePagination(){
    if(page.value == 0)
    {
        page.value = pages.value;
    }
    let comeco =
        per_page.value * (page.value - 1)
    let final =
        per_page.value * page.value
    if (comeco >= filteredData.value.length) {
        page.value = 1
        comeco =
        per_page.value * (page.value - 1)
        final = per_page.value * page.value
    }
    pages.value = Math.ceil(filteredData.value.length / per_page.value)
    display.value = filteredData.value.slice(comeco, final)
}

function sort_table(column: any = null) 
{
    if(column != null)
    {
        if(sort_column.value == column) 
        {
            sort_order.value = !sort_order.value;
        }
        else
        {
            sort_order.value = true
            sort_column.value = column;
        }
    }
    // reduce display data into display
    filteredData.value.sort((a : any, b : any) => 
    {
        if(a[sort_column.value] < b[sort_column.value])
        {
            return sort_order.value ? -1 : 1;
        }
        if(a[sort_column.value] > b[sort_column.value])
        {
            return sort_order.value ? 1 : -1;
        }
        return 0;
    })
    updatePagination()
}

function update_search(value:string)
{
    const filtered = tableData.value.filter((item:any) => {
        for(const key in tableFields.value)
        {
            if(item[tableFields.value[key] as string].toString().toLowerCase().includes(value.toLowerCase()))
            {
                return true;
            }
        }
        return false;
    });
    found.value = filtered.length != 0
    if(filtered.length != 0)
    {
    filteredData.value = filtered
    }
    sort_table()
    updatePagination()
}

function set_page(event:any)
{
    page.value = event;
    updatePagination()
}

updatePagination()
</script>

<template>
    <div class="container">
        <search 
            :found="found" 
            @input="update_search"
        ></search>

        <basic-table 
            :fields="tableFields" 
            :data="display" 
            :padding="per_page" 
            @sort="sort_table" 
            :sort_order="sort_order" 
            :sort_column="sort_column"
        ></basic-table>

        <pagination 
            :page="page" 
            :pages="pages" 
            @input="set_page"
        ></pagination>
    </div>
</template>
