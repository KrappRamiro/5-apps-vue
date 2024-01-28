<!-- App.vue -->
<script>
import InputTag from "./components/InputTag.vue";

export default {
  components: {
    InputTag
  },
  methods: {
    handleOnTagsChange(tags) {
      console.log(tags);
    }
  }
}

</script>

<template>
  <InputTag @onTagsChange="handleOnTagsChange"> </InputTag>
</template>
<style scoped>
</style>
