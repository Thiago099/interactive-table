<script setup lang="ts">
import tableFields from './table_fields.json'
import tableData from './user-data.json' 
import interactiveTable from '@/components/interactive-table/interactive-table.vue'
</script>

<template>
    <interactive-table :table-fields="tableFields" :table-data="tableData" />
</template>
