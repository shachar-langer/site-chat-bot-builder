<template>
  <div class="inline-block border-2 m-2 p-4 max-w-xs">
      <form class="flex flex-col" @submit.prevent="onSubmit">
        <div class="mb-2">
            <label for="name">Block name: </label>
            <input
                v-model="name"
                id="name"
                type="text"
                name="name"
                placeholder="Please insert the block name"
                class="w-full"
            />
        </div>

        <div class="mb-2">
            <label for="block-type">Type: </label>
            <select v-model="blockType" name="block-type" id="block-type" class="w-full">
                <option value="text">Text</option>
                <option value="image">Image</option>
            </select>
        </div>

        <div class="mb-2" v-if="blockType === 'text'">
            <label for="text">Text: </label>
            <input
                id="text"
                type="text"
                name="text"
                placeholder="Please insert your text here"
                class="w-full"
            />
        </div>

        <div class="mb-2" v-if="blockType === 'image'">
            <label for="image">Image: </label>
            <input
                id="image"
                type="file"
                name="imageFile"
                accept="image/png, image/jpeg"
                class="w-full"
            />
        </div>

        <div class="mb-2">
            <label for="action-type">Action: </label>
            <select v-model="actionType" name="action-type" id="action-type" class="w-full">
                <option value="">No action</option>
                <option value="yes-no">Yes/No</option>
                <option value="input">Input</option>
            </select>
        </div>

        <input type="submit" class="py-2"/>
      </form>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const emit = defineEmits(['add'])

const name = ref('')
watch(name, (newName) => console.log(`Name ${newName}`))

const blockType = ref('text')
watch(blockType, (newBlockType) => console.log(`Block type ${newBlockType}`))

const actionType = ref('')
watch(actionType, (newActionType) => console.log(`Action type ${newActionType}`))

const onSubmit = () => {
    console.log('submitting')
    emit('add', {name, blockType, actionType})
}
</script>

<style>

</style>