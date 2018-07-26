<template>
  <div>
    <v-select
      :label="floatLabel"
      :value="model"
      :items="options"
      @input="input"
      item-text="label"
      item-value="value"
      v-model="state"
      :disabled="disable"
      :loading="loading"
    />
    <div class="error-container" v-if="formHandle.$error">
      <span class="errorList" v-for="error in errors" :key="error">* {{ errorMessage[error] }}</span>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return {
      state: ''
    }
  },
  props: ['model', 'name', 'handler', 'formData', 'floatLabel', 'disable', 'options', 'loading'],
  model: {
    prop: 'model',
    event: 'updateModel'
  },
  created(){
    this.state = this.model
  },
  methods: {
    input(){
      this.$emit('updateModel', this.state)
      this.$emit('change')
    }
  },
  watch: {
    'model': function(){
      this.state = this.model
    }
  },
  computed: {
    errors: function () {
      return Object.getOwnPropertyNames(this.formHandle).filter(x => this.formHandle[x] === false && x[0] !== '$')
    },
    formHandle: function () {
      let data = this.name.split('.').reduce((acc, curr) => {
        return acc[curr]
      }, this.handler.form)
      return data
    },
    errorMessage: function () {
      let data = this.name.split('.').reduce((acc, curr) => {
        return acc[curr]
      }, this.formData.errorMessages)
      return data
    }
  }
}
</script>

<style lang="stylus" scoped>
@import '~assets/stylus/variables.styl';
.error-container {
  text-align: right;
  font-size: 12px;
}
.errorList {
  color: $color-red
  display: block;
  width: 100%;
  margin-top: 5px;
}
</style>