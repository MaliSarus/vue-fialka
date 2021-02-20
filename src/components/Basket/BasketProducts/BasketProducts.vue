<template>
    <section class="card">
        <input type="hidden" name="status" value="save">
        <div id="app-list">
            <ul class="list-style buy-list">
                <li class="buy-list__item"
                    v-for="(product, index) in productsData" :key="product.id">
                    <div v-if="product.restored" class="restore-msg">Товар {{product.name}} был
                        удалён из корзины. <a
                                class="item-restore" @click.prevent="restoreProduct(index, product.id)">Восстановить</a>
                    </div>

                    <div v-if="!product.restored" class="card-product__head">
                        <a class="card-item__link"
                           :href="product.link">
                            <img class="card-item__photo card-item__photo--group-card" :src="product.picture">
                        </a>
                        <a class="card-list-product__title"
                           :href="product.link">
                            <h4>{{product.name}}</h4>
                        </a>
                    </div>
                    <div v-if="!product.restored" class="card-product__content">
                        <div class="card-product__price-block">
                            <p class="card_price--all">{{(product.price * product.quantity) | numberFormat}} <span
                                    class="rouble--small">₽</span></p>
                            <p class="card_price--thing">{{product.price | numberFormat}} <span
                                    class="rouble--small">₽</span> за 1
                                шт.</p>
                        </div>
                    </div>

                    <div v-if="!product.restored" class="quantity quantity--card">


                        <button class="quantity__min quantity__min--card" @click.prevent="subQuantity(index)">
                            -
                        </button>
                        <p class="quantity__number-box quantity__number-box--card c-item__num">

                            <span class="quantity__number">{{product.quantity}}</span>

                            <input id="quan" style="display: none" type="number" min="1" max="8" value="1"
                                   data-discount="" data-cost="650" data-id="18355"
                                   onchange="changenum(this);"/>
                        </p>

                        <button class="quantity__plus quantity__plus--card" @click.prevent="addQuantity(index)">
                            +
                        </button>


                    </div>


                    <button class="c-item__delete button--close" :class="{restored: product.restored}" data-id="18355"
                            @click.prevent="deleteProduct(index,product.id)"></button>

                </li>
            </ul>
        </div>
        <div class="card-sum">
            <div class="discont__form" id="discountForm">
                <div id="card_activation_text">
                    <label class="discont__label">
                        Номер дисконтной карты:
                        <input placeholder="номер карты" class="discont__field" id="PERSONAL_WWW" size="30"
                               type="text" name="PERSONAL_WWW" value="">
                    </label>
                </div>
            </div>

            <div class="final-price-block">
                <p class="price">Товары на сумму: <span class="price__sum">{{totalPrice | numberFormat}}<span
                        class="rouble--small"> ₽</span></span></p>
            </div>
        </div>
        <p class="attention">
            <span class="attention__head">Внимание!</span>
            <span class="attention__text">В случае, если Вам необходим определённый производитель, уточните об этом в комментариях.</span>
        </p>

    </section>
</template>

<script>
    import axios from "axios";
    import numberFormat from "@/helpers/filters/numberFormat";

    export default {
        name: "BasketProducts",
        props: ['products', 'totalPrice'],
        computed: {
            productsData: {
                get() {
                    return this.products
                },
                set(value) {
                    this.$emit('update:products', value)
                }
            },
        },
        filters: {
            numberFormat
        },
        methods: {
            restoreProduct(index, id) {
                const product = this.productsData[index];
                product.restored = false;
                axios
                    .post('basket/', {
                        itemID: id,
                        quantity: product.quantity
                    })
            },
            addQuantity(index) {
                this.productsData[index].quantity += 1;
                const productID = this.productsData[index].id;
                const productQuantity = this.productsData[index].quantity;
                this.updateProductRequest(productID, productQuantity);

            },
            subQuantity(index) {
                const product = this.productsData[index];
                product.quantity -= 1;
                if (product.quantity < 1) {
                    product.quantity = 1
                }
                const productID = this.productsData[index].id;
                const productQuantity = this.productsData[index].quantity;
                this.updateProductRequest(productID, productQuantity);

            },
            deleteProduct(index, id) {
                const product = this.productsData[index];
                if (!product.restored) {
                    product.restored = true;
                    axios
                        .delete('basket/', {
                            data: {
                                itemID: id
                            }
                        })
                        .then(response => {
                            console.log(response)
                        });

                } else {
                    this.productsData = this.productsData.filter(product => product.id !== id)
                }

            },
            updateProductRequest(itemID, quantity) {
                axios
                    .put('basket/', {
                        itemID,
                        quantity
                    })
                    .then((response) => {
                        console.log(response);
                    })
            },
        },
        updated() {
            console.log('update');
        },
        mounted() {
            window.arr = ['lalala']
        }
    }
</script>

<style scoped lang="scss">
    .card {
        box-shadow: 0px 5px 25px rgba(167, 167, 167, 0.25);
        border-radius: 5px;
        padding: 30px 20px;
        background: white;
        @media screen and (min-width: 768px) {
            grid-column: 1;
            grid-row: 1;
        }
    }

    .card_price {
        &--all {
            margin-bottom: 5px;
        }

        &--thing {
            margin-top: 5px;
        }
    }

    .attention {
        margin: 0;
        flex-direction: column;
        align-items: flex-start;
        text-align: left !important;

        &__text {
            padding-right: 0;
            margin-top: 5px;
        }
    }

    .buy-list {
        margin-bottom: 10px;
        max-height: 300px;
        overflow-y: auto;
        @media screen and (min-width: 768px){
            max-height: none;
            overflow: auto;
        }
        &__item {
            display: flex;
            flex-flow: column wrap;
            align-items: flex-start;
            position: relative;
            padding-right: 50px;
            padding-top: 10px;
            padding-bottom: 10px;

            &:not(:last-child) {
                margin-bottom: 10px;
            }
        }
    }

    .card-product {
        &__head {
            width: 100%;
            display: flex;

            .card-item__link {
                padding: 0 !important;
            }
        }
    }

    .buy-list .card-product__price-block {
        display: block;
        text-align: left;
    }

    .quantity {
        position: absolute;
        margin-right: 0;
        right: 20px;
        bottom: 20px;
        border-radius: 100px;
        overflow: hidden;
        border: 1px solid #f5e8f0;

        &__min, &__plus {
            border-radius: 0;
            height: 30px;
            width: 30px;
            border: none;
            transition: background-color .2s, color .2s;
        }

        &__min {
            border-right: 1px solid #f5e8f0;
        }

        &__plus {
            border-left: 1px solid #f5e8f0;
        }

        &__number-box {
            height: 30px;
            width: 50px;
            border: none;
            transition: background-color .2s, color .2s;
        }
    }

    .buy-list .button--close {
        position: absolute;
        right: 20px;
        top: 20px;
        padding: 14px;
        border-radius: 50%;
        border: 1px solid #f5e8f0;
        width: auto;
        height: auto;
        background-size: 12px;
        transition: background-color .2s;

        &.restored {
            top: 50%;
            transform: translateY(-50%);
        }

        &:hover {
            background-color: #f5e8f0;
        }
    }

    .card-section {
        .discont {
            &__label {
                font-size: 14px;
            }

            &__field {
                margin-top: 5px;
                display: block;
                margin-right: 0;
                margin-left: 0;
            }
        }

        .final-price-block {
            flex-basis: auto;
            margin: unset;

            .price {
                font-size: 18px;
                margin-top: 10px;
                @media screen and (min-width: 992px) {
                    margin-top: 0;
                }
            }
        }
    }
</style>