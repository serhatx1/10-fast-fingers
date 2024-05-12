<template>
  <div>
    <div>Your wpm = {{ correctWords + wrongWords }}</div>
    <div>Correct words: {{ correctWords }}</div>
    <div>Wrong words: {{ wrongWords }}</div>
  </div>
</template>

<script>
import { defineProps } from 'vue';

export default {
  props: {
    correctWords: {
      type: Number,
      required: true
    },
    wrongWords: {
      type: Number,
      required: true
    }
  }
}
</script>

<style>
</style>
