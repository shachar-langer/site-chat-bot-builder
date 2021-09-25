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
                v-model="text"
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

        <div class="mb-2" v-if="actionType === 'input'">
            <label for="next-block">Next block: </label>
            <select v-model="nextBlockId" name="next-block" id="next-block" class="w-full">
                <option value="">No next block</option>
                <option v-for="(block, index) in allBlocks" :key="index" :value="block.id">{{block.name}}</option>
            </select>
        </div>

        <div v-if="actionType === 'yes-no'">

            <div class="mb-2">
                <label for="yes-label">Left button label: </label>
                <input
                    v-model="yesLabel"
                    id="yes-label"
                    type="text"
                    name="yesLabel"
                    placeholder="Please insert the label for the left button"
                    class="w-full"
                />
            </div>

            <div class="mb-2">
                <label for="next-block-yes">Next block for "yes": </label>
                <select v-model="nextBlockYesId" name="next-block-yes" id="next-block-yes" class="w-full">
                    <option value="">No next block</option>
                    <option v-for="(block, index) in allBlocks" :key="index" :value="block.id">{{block.name}}</option>
                </select>
            </div>

            <div class="mb-2">
                <label for="no-label">Right button label: </label>
                <input
                    v-model="noLabel"
                    id="no-label"
                    type="text"
                    name="noLabel"
                    placeholder="Please insert the label for the right button"
                    class="w-full"
                />
            </div>

            <div class="mb-2">
                <label for="next-block-no">Next block for "no": </label>
                <select v-model="nextBlockNoId" name="next-block-no" id="next-block-no" class="w-full">
                    <option value="">No next block</option>
                    <option v-for="(block, index) in allBlocks" :key="index" :value="block.id">{{block.name}}</option>
                </select>
            </div>
        </div>

        <div v-if="actionType === 'input'">
            <div class="mb-2">
                <label for="inputLabel">Label: </label>
                <input
                    v-model="inputLabel"
                    id="inputLabel"
                    type="text"
                    name="inputLabel"
                    placeholder="Please insert the input field's label"
                    class="w-full"
                />
            </div>

            <div class="mb-2">
                <label for="inputPlaceholder">Placeholder: </label>
                <input
                    v-model="inputPlaceholder"
                    id="inputPlaceholder"
                    type="text"
                    name="inputPlaceholder"
                    placeholder="Please insert the input field's placeholder"
                    class="w-full"
                />
            </div>
        </div>

        <input type="submit" class="py-2"/>
      </form>
  </div>
</template>

<script setup>
import { v4 as uuidv4 } from 'uuid';
import { ref, watch } from 'vue'

const emit = defineEmits(['add'])
const props = defineProps({
    allBlocks: {
        type: Array,
        required: true
    }
})

const name = ref('')
const blockType = ref('text')
const text = ref('')
const inputLabel = ref('')
const inputPlaceholder = ref('')
const actionType = ref('')
const nextBlockId = ref('')
const yesLabel = ref('')
const nextBlockYesId = ref('')
const noLabel = ref('')
const nextBlockNoId = ref('')

const onSubmit = () => {
    const id = uuidv4()
    let action
    switch (actionType.value) {
        case 'input':
            action = {
                type: 'text-input',
                attributes: {
                    inputLabel,
                    inputPlaceholder,
                    nextBlockId: nextBlockId.value
                }
            }
            break
        case 'yes-no':
            action = {
                type: actionType.value,
                attributes: {
                    yes: {
                        label: yesLabel,
                        nextBlockId: nextBlockYesId.value
                    },
                    no: {
                        label: noLabel,
                        nextBlockId: nextBlockNoId.value
                    }
                }
            }
            break
    }

    let content
    switch (blockType.value) {
        case 'text':
            content = {
                type: blockType.value,
                attributes: {
                    text
                }
            }
            break
        case 'image':
            content = {
                type: blockType.value,
                attributes: {
                    fileName: ""
                }
            }
            break
    }

    emit('add', {
        id,
        name: name.value,
        content,
        action
    })
}
</script>
