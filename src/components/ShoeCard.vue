<template>
  <div id="shoe-card">
    <b-card class="overflow-hidden" style="width: 600px; height: 380px; background-color: #F5F5F5">
      <b-row>
        <b-col cols="7">
          <b-card-img :src="item.imgSrc" :alt="item.imgAlt"/>
        </b-col>
        <b-col cols="5" class="d-flex flex-wrap align-items-center">
          <h5 style="width: 100%">{{ item.title }}</h5>
          <b-card-text class="summary">
            {{ item.summary }}
          </b-card-text>
          <b-row class="w-100 justify-content-center">
            <h5 class="m-0" style="color: #333333">{{ calculatePrice }} $</h5>
          </b-row>
        </b-col>
      </b-row>
      <b-row style="padding-bottom: 16px; padding-top: 8px;">
        <b-col cols="7">
          <span v-for="i in item.rating" :key="'checked' + i" class="fa fa-star checked"
                :class="'checked-'+  item.color"/>
          <span v-for="j in 5 - item.rating" :key="'unchecked' + j" class="fa fa-star"/>
        </b-col>
      </b-row>
      <b-row style="padding-bottom: 8px">
        <b-col cols="7" class="text-left " style="font-size: 16px; padding-left: 24px">
          <b class="align-text-top">Sizes</b>
        </b-col>
        <b-col cols="5" class="d-flex justify-content-center">
          <i @click="changeCount('dec')" class="fa fa-minus mx-3 d-flex align-items-center" aria-hidden="true"></i>
          <i id="count">{{ shoeCount }}</i>
          <i @click="changeCount('inc')" class="fa fa-plus mx-3 d-flex align-items-center" aria-hidden="true"></i>
        </b-col>
      </b-row>
      <b-row>
        <b-col cols="7">
          <b-form-group v-slot="{ ariaDescribedby }">
            <b-form-radio-group
                v-model="selectedSize"
                :options="shoeSizes"
                buttons
                :aria-describedby="ariaDescribedby"
                button-variant="outline-dark"
            ></b-form-radio-group>
          </b-form-group>

        </b-col>
        <b-col cols="5">
          <b-button @click="selectedTitle = item.title;addToBasket()" :class="item.color" href="#">Add to basket
          </b-button>
        </b-col>
      </b-row>
    </b-card>
  </div>
</template>

<script>
export default {
  name: "ShoeCard",
  props: {
    item: Object,
  },
  data() {
    return {
      shoeSizes: [5, 6, 7, 8, 9, 10, 11, 12],
      selectedSize: null,
      selectedTitle: "",
      shoeCount: 1,
      // selectedShoeSize: 5
    }
  },
  computed: {
    calculatePrice() {
      if (this.selectedSize > 8) {
        return this.item.price * 1.2
      } else {
        return this.item.price
      }
    }
  },
  watch: {
    selectedSize: function (newVal, oldVal) {
      this.shoeCount = newVal === oldVal ? this.shoeCount : 1
    }
  },
  methods: {
    changeCount(action) {
      if (action === "inc" && this.shoeCount < 10) {
        this.shoeCount += 1;
      } else if (action === "dec" && this.shoeCount > 1) {
        this.shoeCount -= 1;
      }
    },
    addToBasket() {
      if (this.selectedSize != null) {
        let selectedItem = {
          title: this.selectedTitle,
          size: this.selectedSize,
          shoeCount: this.shoeCount,
          price: this.calculatePrice
        }
        this.$emit("basket", selectedItem);
      } else {
        this.$toasted.show("Please select shoe size", {
          theme: "outline",
          position: "top-right",
          duration: 2000,
          iconPack: "fontawesome",
          icon: "warning",
          type: "error"
        });
      }
    }
  }
}
</script>

<style scoped>
#shoe-card {
  padding-bottom: 10px;
  padding-top: 10px;
}

#count {
  width: 32px;
  border-color: lightsteelblue;
  border-style: dashed;
  font-style: normal;
}

.pink {
  background-color: #cca9a8;
}

.blue {
  background-color: #4A9BC6;
}

.checked-pink {
  color: #cca9a8;
}

.checked-blue {
  color: #4A9BC6;
}

.summary {
  font-size: small;
  text-align: start;
}

.btn-shoe-size {
  font-size: 8px;
}

.btn-shoe-size:checked {
  background: #FFFFFF;
  box-shadow: none;
}

.btn-shoe-size:active {
  background: #FFFFFF;
  box-shadow: none;
}

.btn-shoe-size:hover {
  background: #FFFFFF;
}


</style>