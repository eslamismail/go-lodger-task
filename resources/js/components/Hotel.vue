<template>
  <div class="container">
    <div class="row mt-3 justify-content-center">
      <div class="col-md-3 text-center">
        <label for="price" class="col-form-label text-capitalize">price</label>
        <select
          name="price"
          @change="filterPrice()"
          v-model="price"
          id="price"
          class="form-control"
        >
          <option disabled selected :value="null">-Select filter type-</option>
          <option value="h_l">high to low</option>
          <option value="l_h">low to high</option>
        </select>
      </div>
      <div class="col-md-3 text-center">
        <label for="stars" class="col-form-label text-capitalize">stars</label>
        <select
          name="stars"
          @change="filterStars()"
          v-model="stars"
          id="stars"
          class="form-control"
        >
          <option disabled selected :value="null">-Select filter type-</option>
          <option value="h_l">high to low</option>
          <option value="l_h">low to high</option>
        </select>
      </div>
    </div>
    <div class="row mt-5 justify-content-center">
      <div class="col-md-3" v-for="(item, index) in this.mainHotels" :key="index">
        <div class="card">
          <div class="card-header">name : {{item.name}}</div>
          <div class="card-body">
            <p>stars : {{item.stars}}</p>
            <p>high price : {{item.high_price}}</p>
            <p>low price : {{item.low_price}}</p>
          </div>
          <div
            class="card-body"
            v-for="(item, index) in item.rooms"
            :key="index"
          >{{item.code}} - {{item.total}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const _ = require("underscore");
export default {
  props: {
    hotels: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      mainHotels: [],
      price: null,
      stars: null,
    };
  },
  created() {
    this.mainHotels = this.hotels?.hotels ?? [];
    for (let index = 0; index < this.mainHotels.length; index++) {
      const item = this.mainHotels[index];
      const high_room = _.sortBy(item.rooms, function (room) {
        return -parseFloat(room.total);
      });
      const low_room = _.sortBy(item.rooms, function (room) {
        return parseFloat(room.total);
      });
      const high_price = high_room[0].total;
      const low_price = low_room[0].total;
      item["high_price"] = high_price;
      item["low_price"] = low_price;
    }
    // this.mainHotels = this.mainHotels.forEach((item) => {
    //   //   item.high_price = "test";
    // });
  },
  methods: {
    filterPrice() {
      const price = this.price;
      this.stars = null;
      this.mainHotels = _.sortBy(this.mainHotels, function (item) {
        if (price == "h_l") {
          return -parseFloat(item.high_price);
        } else {
          return -parseFloat(item.low_price);
        }
      });
      console.log(this.mainHotels);
    },
    filterStars() {
      this.price = null;
      const stars = this.stars;
      this.mainHotels = _.sortBy(this.mainHotels, function (item) {
        if (stars === "h_l") {
          return -parseInt(item.stars);
        } else {
          return parseInt(item.stars);
        }
      });
    },
  },
};
</script>
