<template>
  <div>
    <v-menu
      ref="menu"
        :close-on-content-click="false"
        v-model="menu"
        :nudge-right="40"
        :return-value.sync="state"
        lazy
        transition="scale-transition"
        offset-y
        full-width
        min-width="290px"
    >
      <v-text-field
        slot="activator"
          v-model="state"
          :label="floatLabel" 
          readonly
          :placeholder="placeholder" 
          :type="type"
          :value="model"
      >
      </v-text-field>
      <v-date-picker v-model="state" @input="$refs.menu.save(state)" @change="input"></v-date-picker>
    </v-menu>
    <div class="error-container" v-if="formHandle.$error">
      <span class="errorList" v-for="error in errors" :key="error">* {{ errorMessage[error] }}</span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      menu: false,
      state: '',
    }
  },
  props: [
    'model',
    'name',
    'handler',
    'formData',
    'floatLabel',
    'placeholder',
    'type'
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
    },
  },
  watch: {
    'model': function(){
      this.state = this.model
    }
  },
  computed: {
    errors: function(){name
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
  font-size: 12px;
}
.errorList {
  color: $color-red
  display: block;
  width: 100%;
  margin-top: 5px;
}
</style>