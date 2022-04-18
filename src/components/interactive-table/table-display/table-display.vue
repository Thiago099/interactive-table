<script setup lang="ts">
import { ref, toRefs } from 'vue'
const props = defineProps<{fields:string, data:object, padding:object, sort_column:string, sort_order:boolean}>()
const { fields, data, padding } = toRefs(props)
function firstUpper(str : string)
{
    return str.charAt(0).toUpperCase() + str.slice(1);
}
</script>
<style scoped lang="less">
@import "./table-display";
</style>

<template>
    <table cellspacing="0">
        <tr>
        <th v-for="title in fields" :key="title" @click="$emit('sort', title)" class="table-header no-select">
            {{ firstUpper(title) }}
            <i
                class="fa fa-fw fa-sort"
                :class="{
                    'fa-sort-amount-asc' : sort_column == title && sort_order == true,
                    'fa-sort-amount-desc': sort_column == title && sort_order == false,
                }"
            ></i>
        </th>
        <th v-if="display_options" >Options</th>
        </tr>
        <tr v-for="row in data" :key="row">
            <td v-for="col in fields" :key="col">
                {{ row[col] }}
            </td>
            <td class="options" v-if="display_options">
                <router-link :to="`/${edit_link}/${row.id}`">
                    <i class="fa fa-edit info hover"></i>
                </router-link>
                <i @click="remove(row)" class="fa fa-trash danger hover"></i>
            </td>
        </tr>
        <tr v-for="i in data.length <= padding ? (padding - data.length) : 0" :key="i">
            <td v-for="col in fields" :key="col" class="row-placeholder no-select">
                *
            </td>
        <td class="row-placeholder no-select" v-if="display_options">*</td>
        </tr>
    </table>
</template>