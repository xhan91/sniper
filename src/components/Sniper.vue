<template>
  <div>
    <textarea v-model="template" name="" id="" cols="30" rows="10" ></textarea>
    <ul>
      <li v-for="variable in varList" :key="variable">
        {{variable}}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'sniper',
  data() {
    return {
      template: '',
      varList: []
    }
  },
  watch: {
    template() {
      this.extractVariables();
    }
  },
  methods: {
    extractVariables() {
      const varSet = new Set();
      const reg = /<(.+?)>/g
      const vars = this.template.matchAll(reg);
      for (const variable of vars) {
        console.log(variable);
        varSet.add(variable[1]);
      }
      this.varList = Array.from(varSet);
    }
  }
}
</script>
