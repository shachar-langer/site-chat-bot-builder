<template>
  <Block :all-blocks="blocks" @add="onAdd"/>
  <pre class="mt-6">{{ blocks }}</pre>
</template>

<script setup>
import { computed, ref } from 'vue'
import Block from '@/components/Block.vue'

const blocks = ref([])

const blockNames = computed(() => blocks.value.map(b => b.name))

const onAdd = (newBlock) => {
    console.log('Adding', newBlock)
    blocks.value.push(newBlock)
}
</script>