<template>
  <div id="app">
    <nav class="navbar is-transparent">
      <div class="navbar-brand">
        <a class="navbar-item" href="https://bulma.io">
          <img src="https://bulma.io/images/bulma-logo.png" alt="Bulma: a modern CSS framework based on Flexbox" width="112" height="28">
        </a>
        <div class="navbar-burger burger" data-target="navbarExampleTransparentExample">
          <span></span>
          <span></span>
          <span></span>
        </div>
      </div>

      <div id="navbarExampleTransparentExample" class="navbar-menu">
        <div class="navbar-start">
          <a class="navbar-item" href="https://bulma.io/">
            Home
          </a>
          <div class="navbar-item has-dropdown is-hoverable">
            <a class="navbar-link" href="https://bulma.io/documentation/overview/start/">
              Docs
            </a>
            <div class="navbar-dropdown is-boxed">
              <a class="navbar-item" href="https://bulma.io/documentation/overview/start/">
                Overview
              </a>
              <a class="navbar-item" href="https://bulma.io/documentation/modifiers/syntax/">
                Modifiers
              </a>
              <a class="navbar-item" href="https://bulma.io/documentation/columns/basics/">
                Columns
              </a>
              <a class="navbar-item" href="https://bulma.io/documentation/layout/container/">
                Layout
              </a>
              <a class="navbar-item" href="https://bulma.io/documentation/form/general/">
                Form
              </a>
              <hr class="navbar-divider">
              <a class="navbar-item" href="https://bulma.io/documentation/elements/box/">
                Elements
              </a>
              <a class="navbar-item is-active" href="https://bulma.io/documentation/components/breadcrumb/">
                Components
              </a>
            </div>
          </div>
        </div>
      </div>
    </nav>
    <div class="container">
      <div style="margin-top: 25px; width: 100%; justify-content: flex-end; display: flex;">
        <b-button type="is-primary" outlined @click="isComponentModalActive = true">New product</b-button>
      </div>
      <div style="margin-top: 25px; width: 100%;">
        <b-table :data="data" :columns="columns"></b-table>
      </div>
    </div>

    <b-modal :active.sync="isComponentModalActive" has-modal-card>
        <form @submit.prevent="saveProduct">
          <div class="modal-card" style="width: auto">
              <header class="modal-card-head">
                  <p class="modal-card-title">New Product</p>
              </header>
              <section class="modal-card-body">
                  <b-field label="Name">
                      <b-input
                          placeholder="Name of the product"
                          v-model="newProduct.name"
                          required>
                      </b-input>
                  </b-field>

                  <b-field label="Description">
                      <b-input maxlength="200" v-model="newProduct.description" type="textarea"></b-input>
                  </b-field>

                  <b-field label="Price">
                      <b-input
                          type="number"
                          v-model="newProduct.price"
                          placeholder="Price of the product"
                          required>
                      </b-input>
                  </b-field>

                  <b-field label="Qty">
                      <b-input
                          type="number"
                          v-model="newProduct.qty"
                          placeholder="Qty of the product"
                          required>
                      </b-input>
                  </b-field>
              </section>
              <footer class="modal-card-foot">
                  <button class="button" type="button" @click="isComponentModalActive = false">Close</button>
                  <button class="button is-primary">Save</button>
              </footer>
          </div>
      </form>
    </b-modal>
  </div>
</template>

<script>

import axios from 'axios';
import NewProductModal from './components/NewProductModal';

export default {
  name: "app",
  components: {
    NewProductModal
  },
  data() {
    return {
      data: [],
      newProduct: {
        name: '',
        description: '',
        price: '',
        qty: ''
      },
      columns: [
        {
          field: "id",
          label: "ID",
          width: "40",
          numeric: true
        },
        {
          field: "name",
          label: "Name"
        },
        {
          field: "description",
          label: "Description"
        },
        {
          field: "price",
          label: "Price",
          centered: true
        },
        {
          field: "qty",
          label: "Qty"
        }
      ],
      isComponentModalActive: false
    };
  },
  mounted() {
    axios.get('http://localhost:5000/product').then(response => {
        this.data = response.data;
    });
  },
  methods: {
    saveProduct() {
      console.log('save product');
      console.log('product', this.newProduct.name);
      
      const { name, description, price, qty } = this.newProduct;
      
      axios.post('http://localhost:5000/product', {
        name,
        description,
        price,
        qty
      }).then(response => {
        console.log('response', response);
        
        if(response.status == 200) {
          this.data.push(response.data);
          this.isComponentModalActive = false;
        }

      }).catch(error => {
        console.log('error', error);
      });
    },
  }
};
</script>

<style>
body, html {
  background-color: #f5f5f5;
  height: 100%;
}
</style>
