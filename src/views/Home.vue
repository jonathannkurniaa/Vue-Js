<template>
  <div class="home">
    <Header></Header>
    <div class="container">
      <div v-for="x in item" :key="x.id" class="card" style="width: 18rem;">
        <img v-bind:src="linkGambar(x.gambar)" class="card-img-top" alt="..." />

        <div class="card-body">
          <h5 class="card-title">{{ x.nama }}</h5>

          <p class="card-text">
            {{ x.harga }}
          </p>
          <button class="btn btn-primary" v-on:click="beli(x.id)">
            Beli
          </button>
          <p class="card-text">
            <small class="text-muted">expired : {{ x.expired }}</small>
          </p>
        </div>
      </div>
    </div>
    <div class="keranjang">
      <hr />
      <h1>KERANJANG</h1>
      <table class="table">
        <thead>
          <tr>
            <th scope="col">#</th>
            <th scope="col">Gambar</th>
            <th scope="col">Nama</th>
            <th scope="col">Quantity</th>
            <th scope="col">Harga</th>
          </tr>
        </thead>
        <tbody v-if="keranjang.length > 0">
          <tr v-for="(y, index) in keranjang" :key="index">
            <td scope="row">{{ index + 1 }}</td>
            <td style="width:30%">
              <img width="30%" :src="linkGambar(y.gambar)" alt="" />
            </td>
            <td>{{ y.nama }}</td>
            <td><input type="number" min="1" v-model="y.qty" /></td>

            <td>{{ totalPerItem(y.harga, y.qty) }}</td>
            <td>
              <button v-on:click="deletee(y.id)" class="btn btn-danger">
                Delete
              </button>
            </td>
          </tr>

          <p>
            Total Harga : <b>{{ totalHarga() }}</b>
          </p>
          <button class="btn btn-primary" v-on:click="checkOut()">
            Checkout
          </button>
        </tbody>
        <h2 style="text-align:center;margin-top:50px;" v-else>
          Keranjang kamu kosong silahkan beli dahulu
        </h2>
      </table>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "../components/Header.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Header,
  },
  data() {
    return {
      item: [],

      keranjang: [],
    };
  },
  async mounted() {
    // this.item = response.data.item;

    // window.localStorage.setItem("data", JSON.stringify(this.item));
    // const storeItems = window.localStorage.getItem("storeItems");

    // if (storeItems != "") {
    //   this.item = JSON.parse(storeItems);
    //   console.log("if")
    // } else {
    //   // await axios
    //     // .get("https://api.npoint.io/ad6f7cb8923b22b047ad")
    //     // .then((response) => (this.item = response.data.item));

    //     const response = await axios.get("https://api.npoint.io/ad6f7cb8923b22b047ad")
    //   this.item = response.data.item
    //     console.log("aaa")

    //     window.localStorage.setItem("storeItems", JSON.stringify(this.item));

    
    // }
    
    const storeItems = localStorage.getItem("storeItems");
    
    if (storeItems) {
      this.item = JSON.parse(storeItems);
      console.log(this.item);
      
    } else {
      // await axios
      //   .get("https://api.npoint.io/ad6f7cb8923b22b047ad")
      //   .then((response) => (this.item = response.data.item));

        const response = await axios.get("https://api.npoint.io/ad6f7cb8923b22b047ad")
        // this.item = response.data.item

        window.localStorage.setItem("storeItems", JSON.stringify(response.data.item));
        console.log("else");
       
    }
  },

  methods: {
    linkGambar: function(value) {
      return "../assets/" + value;
    },
    plusItem: function() {
      console.log("keranjang");
    },
    beli: function(value) {
      const temp = this.item.find(function(eachItem) {
        return eachItem.id === value;
      });
      const temp2 = this.keranjang.find(function(eachKer) {
        return eachKer.id == value;
      });
      if (this.keranjang.length < 1 || !temp2) {
        this.keranjang.push(temp);
      } else {
        if (temp2.id == value) {
          const index = this.keranjang.findIndex(function(indexKer) {
            return indexKer.id == value;
          });
          this.keranjang[index].qty += 1;
        } else {
          this.keranjang.push(temp);
        }
      }
    },

    totalPerItem: function(value, qty) {
      return value * qty;
    },
    deletee: function(value) {
      const index = this.keranjang.findIndex(function(each) {
        return each.id == value;
      });

      this.keranjang.pop(index);
    },
    totalHarga: function() {
      var totalHarga = 0;
      for (let index = 0; index < this.keranjang.length; index++) {
        totalHarga =
          totalHarga + this.keranjang[index].qty * this.keranjang[index].harga;
      }
      return totalHarga;
    },
    checkOut: function() {
      this.keranjang = [];
      alert("Belanja berhasil!");
    },
  },
  computed: {
    gambarKeranjang: function(value) {
      return "../assets/" + value;
    },
  },
};
</script>

<style>
.card {
  float: left;
  margin-left: 40px;
}

.container {
  display: flex;
}
.keranjang {
  margin-top: auto;
}
</style>
