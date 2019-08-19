<template>
  <div>
    <el-row style="margin-bottom: 20px">
      <el-input type="textarea" :autosize="{minRows: 2}" v-model="template">
      </el-input>
    </el-row>
    <el-row v-for="(fillValuesEntry, k) in fillValues" :key="k">
      <el-col :span="varList.length ? Math.floor(20/varList.length) : 2" v-for="variable in varList" :key="variable" style="padding-right: 20px">
        <el-input :placeholder="variable" v-model="fillValuesEntry[variable]">
        </el-input>
      </el-col>
      <el-col :span="2">
        <el-button circle type="success" icon="el-icon-plus" v-on:click="addFillValueEntry(k)"></el-button>
      </el-col>
      <el-col :span="2">
        <el-button circle type="danger" icon="el-icon-minus" v-if="fillValues && fillValues.length > 1" v-on:click="removeFillValueEntry(k)"></el-button>
      </el-col>
    </el-row>
    <el-row style="margin-top: 20px">
      <el-input type="textarea" :autosize="{minRows: 4}" readonly v-model="generatedCommand">
      </el-input>
    </el-row>
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
      let result = '';
      for (const fillValuesEntry of this.fillValues) {
        let t = this.template;
        for (const key of Object.keys(fillValuesEntry)) {
          const reg = new RegExp(`<${key}>`, 'g');
          t = t.replace(reg, `${fillValuesEntry[key]}`);
        }
        result += `${t}\n`;
      }
      this.generatedCommand = result;
    }
  }
}
</script>

<style scoped>
el-input {
  font-family: 'Space Mono', 'Avenir', Helvetica, Arial, sans-serif;
  font-size: 14px;
}
</style>