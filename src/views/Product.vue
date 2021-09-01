<template>
    <div class="home">
        <HeaderShayna />
        
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
                                <div class="product-thumbs" v-if="productDetails.gallery.length > 0">
                                    <carousel :nav="false" :dots="false" class="product-thumbs-track ps-slider">
                            
                                        <div v-for="pic in productDetails.gallery" :key="pic.id" class="pt" @click="changeImage(pic.photo)" :class="pic.photo == gambar_default ? 'active' : '' ">
                                            <img :src="pic.photo" alt="" />
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
                                        <p v-html="productDetails.description"></p>
                                        <h4>${{ productDetails.price }}</h4>
                                    </div>
                                    <div class="quantity">
                                        <a href="shopping-cart.html" class="primary-btn pd-cart">Add To Cart</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        <!-- Product Shop Section End -->
        <RelatedShayna />
        <FooterShayna />
    </div>
</template>

<script>
// @ is an alias to /src
import HeaderShayna from '../components/HeaderShayna.vue';
import FooterShayna from '../components/FooterShayna.vue';
import RelatedShayna from '../components/RelatedShayna.vue';
import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
    name: 'Home',
    components: {
        HeaderShayna,
        FooterShayna,
        carousel,
        RelatedShayna
    },
    data (){
        return {
            gambar_default: "",
            thumbs: [
                "img/mickey1.jpg",
                "img/mickey2.jpg",
                "img/mickey3.jpg",
                "img/mickey4.jpg"
            ],
            productDetails: []
        }
    },
    methods: {
        changeImage(urlImg){
            this.gambar_default = urlImg;
        },
        setData(data){
            this.productDetails = data;
            this.gambar_default = data.gallery[0].photo;
        }
    },
    mounted() {
        axios
            .get("http://0.0.0.0/api/products", {params: {
                id: this.$route.params.id
            }})
            .then(res => (this.setData(res.data.data)))
            .catch(err => console.log(err));
    }
}
</script>

<style scoped>
.product-thumbs .pt{
    margin-right: 12px;
}
</style>