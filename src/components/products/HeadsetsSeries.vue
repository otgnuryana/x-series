<template>
<div>
    <!-- Headsets Banner Section Begin -->
    <section class="women-banner spad" id="headset">
        <div class="container-fluid">
            <div class="row">
                <h3>Headsets</h3>
               <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :autoplay="true" :dots="false" :nav="false">
                        <div class="product-item" v-for="itemProduct in products" :key="itemProduct.id">
                            <div class="pi-pic">
                                <img v-bind:src="'https://backend.otg-web.site/storage/' + itemProduct.galleries[0].photo" alt="" />
                                <ul class="sm-null">
                                    <li class="w-icon active">
                                        <a @click="saveKeranjang(itemProduct.id, itemProduct.name, itemProduct.price, itemProduct.galleries[0].photo)"
                                        href="#"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view">
                                        <router-link v-bind:to="'/product/'+itemProduct.id">+ Quick View</router-link>
                                    </li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">Steelseries</div>
                                <router-link v-bind:to="'/product/'+itemProduct.id">
                                    <h5>{{ itemProduct.name }}</h5>
                                </router-link>
                                <div class="product-price">
                                    ${{ itemProduct.price }}.00
                                    <span v-if="itemProduct.price > 200">${{ parseInt(itemProduct.price) + 100 }}.00</span>
                                </div>
                            </div>
                        </div>
                    </carousel>
                </div>
                <div class="col-lg-12" v-else>
                    <p>Produk belum tersedia untuk saat ini</p> 
                </div>
            </div>
        </div>
    </section>
    <!-- Headsets Banner Section End -->

</div>
</template>

<script>
import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
    name: "HeadsetsSeries",
    components: {
        carousel,
    },
    data() {
        return {
            products: [],
            keranjangUser: []
        };
    },
    methods: {
        saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {
            let productStored = {
                "id" : idProduct,
                "name" : nameProduct,               
                "price" : parseInt(priceProduct),               
                "photo" : photoProduct,               
            }

            this.keranjangUser.push(productStored);
            const parsed = JSON.stringify(this.keranjangUser);
            localStorage.setItem('keranjangUser', parsed);       

            window.location.reload();
        },
    },
    mounted() {
            if (localStorage.getItem('keranjangUser')) {
                try {
                    this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
                } catch(e) {
                    localStorage.removeItem('keranjangUser');
                }
            }
            axios
            .get("https://backend.otg-web.site/api/products?type=headset")
            .then(res => (this.products = res.data.data.data))
            // eslint-disable-next-line no-console
            .catch(err => console.log(err));
    },
};
</script>

<style scoped>
.product-item {
    margin-right: 25px;
}

.pi-pic img {
    max-height: 454px;
}
@media only screen and (max-width: 479px){
    .sm-null {
        opacity: 0;
    }
}
</style>
