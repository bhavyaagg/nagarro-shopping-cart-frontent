<template>
  <div class="col-4 card p-3">
    <h4>{{name}}</h4>
    <div>{{manufacturer}}</div>
    <div><b>Rs. {{price}}</b></div>
    <div class="row">
      <div class="col">
        <button v-on:click="incProduct(productId)" class="btn btn-primary">+</button>
      </div>
      <div v-bind:id="productId" class="col d-flex align-items-center justify-content-center">
        {{(qtyObj?qtyObj.qty:0)}}
      </div>
      <div class="col">
        <button v-on:click="decProduct(productId)" class="btn btn-primary">-</button>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    name: "Product",
    props: {
      name: String,
      manufacturer: String,
      price: Number,
      productId: Number,
      qtyObj: Object
    },
    methods: {
      incProduct: function (productId) {
        axios.post('http://localhost:2678/api/cart/incProduct', {
          name: window.localStorage.getItem("name"),
          password: window.localStorage.getItem("password"),
          productId: productId
        }).then((response) => {
          if (response.data.success) {
            let el = document.getElementById(productId);
            el.innerText = parseInt(el.innerText) + 1;
          }
        })
      },
      decProduct: function (productId) {
        axios.post('http://localhost:2678/api/cart/decProduct', {
          name: window.localStorage.getItem("name"),
          password: window.localStorage.getItem("password"),
          productId: productId
        }).then((data) => {
          if (data.data.success) {
            let el = document.getElementById(productId);
            el.innerText = parseInt(el.innerText) - 1;
          } else {
            // eslint-disable-next-line
            console.log("Error")
          }
        })
      }
    }
  }
</script>

<style scoped>

</style>
