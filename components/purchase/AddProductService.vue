<template>
  <section>
    <header>
      <h2>Add a Product or Service</h2>
      <p>Products and services that you buy from vendors are used as items on Bills to record those purchases, and the ones that you sell to customers are used as items on Invoices to record those sales.</p>
    </header>
    <main>
      <v-form @submit.prevent="$v.$touch()"> 
        <custom-input v-model='form.productServiceName' name="productServiceName" type="text" :formData="form" :handler="$v" floatLabel="Name"/>
        <v-textarea
          label="Description"
          v-model="form.description"
        ></v-textarea>
        <custom-input v-model='form.price' name="price" type="text" :formData="form" :handler="$v" floatLabel="Price"/>
        <v-btn @click="add()">Add</v-btn>
      </v-form>
    </main>
  </section>
</template>

<script>
import { required, numeric } from 'vuelidate/lib/validators';

export default {
  data(){
    return {
      form: {
        productServiceName: '',
        description: '',
        price: '', 
        errorMessages: {
          productServiceName: {
            required: 'Required'
          },
          price: {
            required: "Required",
            numeric: "Enter numbers"
          }
        }
      }
    }
  },
  validations: {
    form: {
      productServiceName: {
        required
      },
      price: {
        required,
        numeric
      }
    }
  },
  methods: {
    add() {
      this.$v.$touch();
      if (this.$v.form.$error) {
        return true
      } else {
      }
    }
  }
}
</script>