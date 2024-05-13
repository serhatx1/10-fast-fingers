<template>
  <div>
    <div class="resultDiv">Your wpm = {{ wpm }}</div>
    <div class="resultDiv">Correct words: {{ correctWords }}</div>
    <div class="resultDiv">Wrong words: {{ wrongWords }}</div>
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
    },
    wpm: {
      type: Number,
      required: true
    }
  }
}
</script>

<style>
.resultDiv{
  padding: 5px;

}
</style>
