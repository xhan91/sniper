<template>
  <div>
    <div>
      <textarea v-model="template" cols="200" rows="10"></textarea>
    </div>
    <div v-for="(fillValuesEntry, k) in fillValues" :key="k">
      <span v-for="variable in varList" :key="variable" style="padding-right: 20px">
        <label for="variable" style="padding-right:5px">{{variable}}</label>
        <input type="text" :id="variable" v-model="fillValuesEntry[variable]">
      </span>
      <button v-if="fillValues && fillValues.length > 1" v-on:click="removeFillValueEntry(k)">-</button>
      <button v-on:click="addFillValueEntry(k)" >+</button>
    </div>
    <div style="margin-top: 20px">
      <textarea readonly v-model="generatedCommand" cols="200" rows="50"></textarea>
    </div>
  </div>
</template>

<script>
export default {
  name: 'sniper',
  data() {
    return {
      template: '',
      varList: [],
      fillValues: [], // [{key1: value1...}]
      generatedCommand: ''
    }
  },
  watch: {
    template() {
      this.reset();
    },
    fillValues: {
      deep: true,
      handler() {
        this.generateCommand();
      }
    }
  },
  methods: {
    reset() {
      this.extractVariables();
      this.$set(this.fillValues, 0, {})
    },
    addFillValueEntry(k) {
      this.fillValues.splice(k+1,0,{});
    },
    removeFillValueEntry(k) {
      this.fillValues.splice(k,1);
    },
    extractVariables() {
      const varSet = new Set();
      const reg = /<(.+?)>/g
      const vars = this.template.matchAll(reg);
      for (const variable of vars) {
        varSet.add(variable[1]);
      }
      this.varList = Array.from(varSet);
    },
    generateCommand() {
      console.log('I run');
      let result = '';
      for (const fillValuesEntry of this.fillValues) {
        let t = this.template;
        for (const key of Object.keys(fillValuesEntry)) {
          const reg = new RegExp(`<${key}>`, 'g');
          t = t.replace(reg, `${fillValuesEntry[key]}`);
        }
        result += `${t}\n\n`;
      }
      this.generatedCommand = result;
    }
  }
}
</script>

<style scoped>
textarea {
  font-family: 'Space Mono', 'Avenir', Helvetica, Arial, sans-serif;
  font-size: 14px;
}
</style>