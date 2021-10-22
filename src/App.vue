<template>
  <div id="app">
    <b-navbar variant="faded" type="light" style="background-color: #F5F5F5">

      <b-navbar-brand href="#">
        <img src="@/assets/logo.png" width="48px" height="48px" class="d-inline-block align-top" alt="Kitten">
      </b-navbar-brand>

      <b-collapse is-nav>
        <b-navbar-nav class="ml-auto">
          <b-nav-item-dropdown right>
            <template #button-content>
              <i class="fa fa-shopping-basket" aria-hidden="true"/>
              <b-badge class="mx-3" size="lg" variant="light">{{ totalItems }}</b-badge>
            </template>
            <b-dropdown-form style="width: 450px" v-for="(item, index) in selectedItems" :key="'selected item ' + index"
                             class="d-flex align-items-center">
              <b-row class="m-0 justify-content-center">
                <b-col cols="3" class="align-items-center">
                  {{ item.title }}
                </b-col>
                <b-col cols="1" class="text-center">
                  {{ item.size }}
                </b-col>
                <b-col class="text-center" cols="4">
                  {{ item.price * item.shoeCount }} $
                </b-col>
                <b-col cols="3" class="d-flex align-items-center">
                  <span @click="item.shoeCount > 1 ? item.shoeCount-- : deleteItem(index)" class="fa fa-minus mx-3 "
                        aria-hidden="true"></span>
                  <span>{{ item.shoeCount }}</span>
                  <span @click="item.shoeCount++" class="fa fa-plus mx-3 " aria-hidden="true"></span>
                </b-col>
                <b-col cols="1">
                  <span role="button" class="fa fa-times mx-3" @click="deleteItem(index)"></span>
                </b-col>
              </b-row>
            </b-dropdown-form>
            <b-col class="text-center">
              Total: {{calculateTotalPrice}}$
            </b-col>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>

    </b-navbar>
    <b-row class="justify-content-around p-3">
      <ShoeCard v-for="(product, i) in productList" :key="'product index'+i" :item="product" @basket="addToBasket"/>
    </b-row>
  </div>
</template>

<script>
import ShoeCard from "@/components/ShoeCard";
import {products} from "@/assets/products";

export default {
  name: 'App',
  components: {
    ShoeCard
  },
  data() {
    return {
      productList: products,
      selectedItems: []
    }
  },
  computed: {
    calculateTotalPrice() {
      let totalPrice = 0
      if(this.selectedItems.length > 0) {
        this.selectedItems.forEach(item => {
          totalPrice += item.price
        })
      }
      return totalPrice
    },
    totalItems() {
      let totalItemCount = 0;
      this.selectedItems.forEach(item => {
        totalItemCount += item.shoeCount
      })
      return totalItemCount
    }
  },
  methods: {
    addToBasket(e) {
      let isAlreadySelected = this.selectedItems.find(item => item.size === e.size && item.title === e.title)
      isAlreadySelected == null ? this.selectedItems.push(e) : isAlreadySelected.shoeCount += e.shoeCount
      console.log(this.selectedItems)
      this.$toasted.show("Item added", {
        theme: "outline",
        position: "top-right",
        duration: 2000,
        iconPack: "fontawesome",
        icon: "shopping-basket",
        type: "success"
      });
    },
    deleteItem(index) {
      this.selectedItems.splice(index, 1)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

.navbar {
  padding-left: 20px;
  padding-right: 20px;
}
</style>
