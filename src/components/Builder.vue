<template>
  <Block @add="onAdd"/>
  <pre class="mt-6">{{block}}</pre>
</template>

<script setup>
import { computed, ref } from 'vue'
import Block from '@/components/Block.vue'

const blocks = ref([])

// TODO - pass the blockNames to the block and add the option to attach blocks
const blockNames = computed(() => blocks.value.map(b => b.name))

const onAdd = (newBlock) => {
    console.log('Adding', newBlock)
    blocks.value.push(newBlock)
}
</script>