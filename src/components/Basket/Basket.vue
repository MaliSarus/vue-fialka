<template>
    <div>
        <div class="basket__head">
            <h1 class="section-title">Корзина</h1>
            <div class="favorites__reset" @click="clearCart">Очистить корзину</div>
        </div>
        <div class="error-message" v-if="products.length <= 0">Корзина пуста</div>
        <form v-else action="" method="post" id="form_basket" name="formbasket" onsubmit="return false;">
            <BasketProducts :products.sync="productsData" :totalPrice="totalPrice"/>

            <div class="basket__contact">
                <div class="basket__contact-content">
                    <BasketContacts :is-verify.sync="isBuyButtonActive" :deliveryCost.sync="deliveryData"/>
                </div>
            </div>


            <div class="item-total">
                <div class="item-frame item-frame--border item-frame--total">
                    <div class="card-register-form">
                        <label class="buy__checkbox-label">
                            <div class="checkbox__decor checkbox__decor--active">
                            </div>
                            <input class="buy__checkbox" type="checkbox" name="card-checkbox"
                                   id="card-checkbox-personal" checked="">
                            Нажимая на кнопку "Оформить заказ", Вы даете согласие на обработку <a
                                class="discount__link"
                                href="http://sotbisite.beget.tech//about/pravovaya_informatsiya/polozhenie_o_politike_konfidentsialnosti_internet/"
                                target="_blank" rel="noopener">персональных данных</a> и соглашаетесь с условиями <a
                                class="discount__link" href="http://sotbisite.beget.tech//delivery/" target="_blank"
                                rel="noopener">доставки и бронирования.</a>
                        </label>

                        <p class="price price--buy price--bold">К оплате:&nbsp;&nbsp;<span
                                class="price__total total-goods__sum">{{totalPrice | numberFormat}} <span
                                class="rouble--small">₽</span></span></p>

                        <input type="submit" value="Оформить заказ"
                               class="oformit c-button buy-button"
                               :class="{
                                    'buy-button__disabled': !isBuyButtonActive
                               }"
                               :disabled="!isBuyButtonActive"
                        />

                    </div>
                </div>
            </div>
        </form>
    </div>
</template>

<script>
    import BasketProducts from "@/components/Basket/BasketProducts/BasketProducts";
    import BasketContacts from "@/components/Basket/BasketContacts/BasketContacts";
    import numberFormat from "@/helpers/filters/numberFormat";
    import axios from 'axios'


    export default {
        name: "Basket",
        data(){
            return{
                isBuyButtonActive: false,
            }
        },
        components: {BasketContacts, BasketProducts},
        model: {
            prop: 'products',
            event: 'products-change'
        },
        filters: {
            numberFormat
        },
        props: ['products', 'totalPrice', 'delivery'],
        methods: {
            clearCart() {
                this.productsData = [];
                axios
                    .delete('basket/');
            },
        },
        computed: {
            deliveryData:{
                get() {
                    return this.delivery
                },
                set(value) {
                    this.$emit('update:delivery', value)
                }
            },
            productsData: {
                get() {
                    return this.products
                },
                set(value) {
                    this.$emit('products-change', value)
                }
            },
        },
    }
</script>

<style scoped lang="scss">
    #form_basket {
        display: grid;
        grid-template-columns: 100%;
        grid-template-rows: auto;
        gap: 20px;
        @media screen and (min-width: 768px) {
            grid-template-columns: 1fr 1fr;
        }
        @media screen and (min-width: 992px) {
            grid-template-columns: 60% 1fr;
        }
    }

    .item-total {
        grid-column: 1;
        @media screen and (min-width: 768px) {
            grid-column: span 2;
        }

        .price__total {
            margin-left: 10px;
        }
    }

    .basket__contact {
        grid-row: 2;
        @media screen and (min-width: 768px) {
            grid-row: 1;
        }

        &-content {
            @media screen and (min-width: 768px) {
                position: sticky;
                top: 0;
            }
            @media screen and (min-width: 1025px) {
                top: 140px;
            }
        }
    }

    .card-section .item-frame--border .card-register-form .price--buy {
        line-height: 1;
    }

</style>