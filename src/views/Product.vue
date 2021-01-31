<template>
  <div class="Product">
  
    <HeaderRj />
      <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom">
                                <img class="product-big-img" :src="gambar_default" alt="" />
                            </div>
                            <div class="product-thumbs" v-if="productDetails.galleries">
                                <carousel :dots="false" :nav="false" class="product-thumbs-track ps-slider">
                                    <div 
                                    v-for="ss in productDetails.galleries"
                                    :key="ss.id"
                                    class="pt"
                                    @click="changeImage(ss.photo)" :class="ss.photo == gambar_default ? 'active' : '' ">
                                        <img :src="ss.photo" alt="" />
                                    </div>

                                </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details text-left">
                                <div class="pd-title">
                                    <span>{{ productDetails.type }}</span>
                                    <h3>{{ productDetails.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p v-html="productDetails.description "> </p>
                                    <h4>${{ productDetails.price }}</h4>
                                </div>
                                <div class="quantity">
                                    <router-link to="/cart">
                                    <a  @click="saveKeranjang(productDetails.id,productDetails.name,productDetails.price,productDetails.galleries[0].photo)"  href="" class="primary-btn pd-cart">Add To Cart</a>
                                    </router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedRj />
    <FooterRj />

  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderRj from '@/components/HeaderRj.vue';
import carousel from 'vue-owl-carousel';
import RelatedRj from '@/components/RelatedRj.vue';
import FooterRj from '@/components/FooterRj.vue';

import axios from "axios";


export default {
name: 'Product',
components: {
    HeaderRj,
    RelatedRj,
    FooterRj,
    carousel
},
data() {
    return {
        gambar_default: "",
        // thumbs: [
        //     "img/mickey1.jpg",
        //     "img/mickey2.jpg",
        //     "img/mickey3.jpg",
        //     "img/mickey4.jpg"
        // ],
        productDetails: [],
        keranjangUser: []
                // id: this.$route.params.id
    }
},
methods: {
    changeImage(urlImage) {
        this.gambar_default = urlImage;
        // //eslint-disable-next-line no-console
        // console.log(this.id);
    },
    setDataPicture(data){
        //replace object productDetails dengan data dari APi
        this.productDetails = data ;
        //replace value gambar default dengan data dari API  (galleries)
        this.gambar_default = data.galleries[0].photo;
    },
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
    }
},

mounted() {
    if (localStorage.getItem('keranjangUser')) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
      } catch(e) {
        localStorage.removeItem('KeranjangUser');
      }
    }
        axios
        .get("http://shayna-backend.belajarkoding.com/api/products",{
            params: {
                 id: this.$route.params.id
            }
        })
        .then(res => (this.setDataPicture(res.data.data)))
        //eslint-disable-next-line-no-console
        .catch(err => console.log(err));
        }
}
</script>

<style scoped>
.product-thumbs .pt{
    margin-right: 14px;
}
</style>
