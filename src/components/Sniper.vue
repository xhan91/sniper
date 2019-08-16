<template>
  <textarea v-model="template" name="" id="" cols="30" rows="10" ></textarea>
</template>

<script>
export default {
  name: 'sniper',
  data() {
    return {
      template: ''
    }
  },
  watch: {
    template() {
      return null;
    }
  },
  method: {
    extractVariables() {
      
    }
  }
}
</script>
