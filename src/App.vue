<template>
    <div id="app">
        <main class="maincontent card-section ">
            <div class="container">
                <Loader v-if="loading"/>
                <Basket v-else v-model="products" :totalPrice="totalPrice" :delivery.sync="delivery" @scroll="scroll"/>

                <div id="modal-order" style="display: none;">
                    <div class="modal-order" style="max-width:400px">
                        <div class="heading">Ваш заказ принят</div>
                        <span class="modal-close">×</span>
                        <div class="content">Заказ <span class="order-number"></span> принят. Обработку заказа выполняет
                            аптека-партнёр <a href="http://sotbisite.beget.tech/https://aptekafialka.com"
                                              target="_blank">aptekafialka.com</a>.
                            В ближайшее время ожидайте звонка менеджера аптеки-партнёра для уточнения деталей заказа.
                            Запомните номер заказа, он Вам потребуется для уточнения деталей.<br><br><strong
                                    style="font-size:20px;">Обработку и доставку осуществляет аптека-партнёр</strong><a
                                    href="http://sotbisite.beget.tech/https://aptekafialka.com" target="_blank"><img
                                    src="http://sotbisite.beget.tech//local/templates/redisigne/img/apteka-logo.svg"
                                    style="display:inline;width:100%;"></a></div>

                    </div>
                </div>

                <div id="test_delivery_list" hidden></div>
            </div>
        </main>
    </div>
</template>

<script>
    import Loader from "@/components/UI/Loader";
    import Basket from "@/components/Basket/Basket";
    import axios from 'axios'


    export default {
        name: 'App',
        components: {
            Basket,
            Loader
        },
        data() {
            return {
                products: [],
                delivery: 0,
                loading: true
            }
        },
        methods: {
            scroll(){
                console.log('scroll');
            },
            fetchProducts() {
                axios
                    .get('basket/')
                    .then(response => {
                        this.products = response.data.itemsList
                            .map((product) => {
                            return {
                                ...product,
                                restored: false
                            }
                        });
                        this.loading = false;
                    })
            },

        },
        computed: {
            productsAmount() {
                return this.products.length
            },
            totalPrice() {
                return this.delivery + this.products.reduce((accumulation, product) => product.price * product.quantity  + accumulation, 0);
            }
        },
        created() {
            setInterval(() => {
                this.fetchProducts()
            }, 3000)
        }
    }
</script>

<style>
</style>
