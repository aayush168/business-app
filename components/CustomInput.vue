<template>
  <div>
    <v-text-field 
      :label="floatLabel" 
      :value="model" 
      @input="input()"
      v-model="state"
      :placeholder="placeholder" 
      :type="type" 
      :multi-line="type==='textarea'? true: false"
      :disabled="disabled"
      rows="2"
    />
    <div class="error-container" v-if="formHandle.$error">
      <span class="errorList" v-for="error in errors" :key="error">* {{ errorMessage[error] }}</span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      state: '',
    }
  },
  props: [
    'model',
    'type',
    'name',
    'handler',
    'formData',
    'floatLabel',
    'placeholder',
    'disabled'
  ],
  model: {
    prop: 'model',
    event: 'updateModel'
  },
  created() {
    this.state = this.model;
  },
  methods: {
    input() {
      this.$emit('updateModel', this.state);
      this.$emit('change')
    }
  },
  watch: {
    'model': function(){
      this.state = this.model
    }
  },
  computed: {
    errors: function(){
      return Object.getOwnPropertyNames(this.formHandle).filter(x => {
        return this.formHandle[x]===false&&x[0]!=='$';
      });
    },
    formHandle: function () {
      let data = this.name.split('.').reduce((acc, curr) => {
        return acc[curr];
      }, this.handler.form);
      return data;
    },
    errorMessage: function(){
      let data = this.name.split('.').reduce((acc, curr) => {
        return acc[curr];
      }, this.formData.errorMessages);
      return data;
    }
  }
}
</script>

<style lang="stylus" scoped>
@import '~assets/stylus/variables.styl';
.error-container {
  text-align: right;
  // color: #db2828;
  font-size: 12px;
  /* width: 100%; */
}
.errorList {
  color: $color-red
  display: block;
  width: 100%;
  margin-top: 5px;
}
</style>