<script setup>
    const props = defineProps({
        value: String,
        id: Number
    })

</script>
<template>
    <button className="square" @click="$emit('handleClick', id)">{{value}}</button>
</template>