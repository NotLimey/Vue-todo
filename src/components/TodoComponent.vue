<template>
    <div :id="`todo_` + index" class="w-full bg-green-500 rounded-sm py-2 px-4 mb-4 flex justify-between" @dblclick="$emit('editTodo')" ref="todos">
        <input 
          :value="todo" 
          type="text" 
          class="bg-transparent border-none p-1 remove-outline-input text-sm uppercase font-semibold text-black"
          @input="e => $emit('update', e)"
        >
        <TrashIcon class="h-6 w-6 cursor-pointer text-black" @click="$emit('remove')" />
      </div>
</template>

<script>
import { TrashIcon } from '@heroicons/vue/solid';

export default {
    props: ["todo", "index"],
    components: {
        TrashIcon
    }
}
</script>
