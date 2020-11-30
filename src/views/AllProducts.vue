<template>
  <div class="allproducts">
    <HeaderShayna />
    <HeroShayna />
    <section class="women-banner spad">
        <div class="container">
            <div class="row">
                <div class="col-md-4" 
                
                v-for="itemProduct in products" v-bind:key="itemProduct.id" >
                   <div class="product-item card">
                            <div class="pi-pic card-body">
                                <img v-bind:src="itemProduct.galleries[0].photo" alt="" />
                                <ul>
                                    <li class="w-icon active">
                                      <a @click="saveKeranjang(itemProduct.id, itemProduct.name, itemProduct.price, itemProduct.galleries[0].photo)" href="#"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    
                                    <li class="quick-view">
                                        <router-link v-bind:to="'/product/'+itemProduct.id">+ Quick View</router-link>
                                    </li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{itemProduct.type}}</div>
                                <router-link v-bind:to="'/product/'+itemProduct.id">
                                    <h5>{{itemProduct.name}}</h5>
                                </router-link>
                                <div class="product-price">
                                    Rp.{{itemProduct.price}}
                                    <span>Rp. 200.000</span>
                                </div>
                            </div>
                    </div>
                        
                
                </div>

                <!-- <div class="col-lg-12 mt-5" v-else>
                    <p>
                        Produk Terbaru Belum Tersedia Untuk Saat Ini
                    </p>

                </div> -->

            </div>
        </div>
    </section>
    <!-- Women Banner Section End -->
    
    <FooterShayna />


  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'
import HeaderShayna from '@/components/HeaderShayna.vue';
import HeroShayna from '@/components/HeroShayna.vue';
import FooterShayna from '@/components/FooterShayna.vue';
import axios from 'axios';


export default {
  name: 'allproducts',
  components: {
    HeaderShayna,
    HeroShayna,
    
    FooterShayna
  },
  data(){
        return{
            products: [],
            keranjangUser: []
        };
    },
    methods: {
        //setDataPicture(data) {
        // replace object productDetail dengan data dari API
       // this.productDetails = data;
   // },
        saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct){

        var productStored = {
            "id": idProduct,
            "name": nameProduct,
            "price": priceProduct,
            "photo": photoProduct
        }
        this.keranjangUser.push(productStored);
        const parsed = JSON.stringify(this.keranjangUser);
        localStorage.setItem('keranjangUser', parsed);

        window.location.reload();
    }
    },
    mounted(){
        if (localStorage.getItem('keranjangUser')) {
        try {
            this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
        } catch(e) {
            localStorage.removeItem('keranjangUser');
        }
        }

        axios
        .get("http://shayna-backend.belajarkoding.com/api/products")
        .then(res => (this.products = res.data.data.data))
        //eslint-disable-next-line no-console
        .catch(err => console.log(err));
    }
};
</script>

<style scoped>
.product-item {
    margin: 10px;

}
</style>