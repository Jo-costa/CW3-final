<template>
    <div class="checkout-page">

        <div class="return-container">
            <button class="go-back" @click="showProducts"><font-awesome-icon icon="fa-solid fa-arrow-left" /></button>
        </div>
        <div class="user-info-baskt-info-container">
            <div class="details-container">

                <div class="user-details">
                    <h2>Enter your details</h2>


                    <form class="options" name="user-data">
                        <div class="first-name-container">
                            <label for="first-name">Name</label>
                            <input type="text" v-model="order.firstName" id="first-name" name="first_name"
                                placeholder="First name">
                        </div>


                        <div class="address-container">
                            <label for="phone">Phone Number</label>
                            <input type="tel" v-model="order.phone" class="phone" name="phone" id="phone"
                                placeholder="Phone Number">
                        </div>



                        <div class="place-order">
                            <button type="submit" @click.prevent="placeOrderCheckoutPage" class="place-order-btn"
                                v-if="isValid">
                                Place order</button>

                            <button class="place-order-btn-disabled" v-else disabled="disabled">Place order</button>
                        </div>

                    </form>

                </div>





            </div>

            <div class="details-confirmation">
                <div class="basket-container" v-if="cartItemCount > 0">
                    <div class="basket">
                        <div class="basket-title">
                            <h2>Basket</h2>
                        </div>
                        <div class="item" v-for="product in cart">
                            <div class="product-info">
                                <img v-bind:src="getProdImg(product)" width="40px" height="40px">
                                <p class="item-name">{{ product.subject }}</p>
                            </div>
                            <form action="" name="item-id">
                                <input type="hidden" :value="product.id">
                                <input type="hidden" :value="product.qty">
                                <input type="hidden" v-model="basketForm.numSpaces">
                                <!-- :value="product.qty" -->

                            </form>
                            <div class="qty-management">
                                <button class="item-decrease" @click="decreaseQty(product)"><font-awesome-icon
                                        icon="fa-solid fa-minus" /></button>
                                <p class="item-qty">Qty <span>{{ product.qty }}</span></p>
                                <button class="item-add" @click="increaseQty(product)"><font-awesome-icon
                                        icon="fa-solid fa-plus" /></button>
                                <button class="item-remove" @click="removeAllMatching(product)"><font-awesome-icon
                                        icon="fa-solid fa-trash" /></button>
                            </div>
                        </div>

                        <button class="removeAllbtn" @click="removeAll(cart)">Remove all</button>
                    </div>
                </div>



                <div class="details-confirmation" v-else>
                    <div class="basket">
                        <div class="basket-title">
                            <h2>Basket</h2>
                        </div>
                        <div class="item item-empty">
                            <span>Cart is empty</span>
                        </div>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { computed } from 'vue';




export default {

    props: ['serverURLProp', "order", "products", "addToCart", "cartCount", "currentView", "showCheckOut",
        "canAddToCart", "cartItemCount", "searchResults", "cart", "getProdImg",
        "getProdID", "removeAlItems", "decrease", "increase", "updateBasketInfo", "placeOrder", "basketForm"],

    data() {
        return {

            valid: false, //valid if name last name and phone number has been filled

        }

    },

    methods: {


        increaseQty(product) {
            this.$emit("inc", product)
        },

        decreaseQty(product) {
            this.$emit("dec", product)
        },

        showProducts() {
            this.$emit("show-products");
            console.log("Clicked");
        },

        placeOrderCheckoutPage() {

            this.$emit("place-order")
        },


        removeAllMatching(product) {

            this.$emit("remove-all-matching", product)
        },

        removeAll(cart) {
            this.$emit("remove-all-from-cart", cart)
        },


    },

    computed: {
        isValid: function () {


            if ((this.order.firstName && this.order.phone !== null)) {
                this.valid = true;

            } else {
                this.valid = false;
            }

            return this.valid;
        },







    }
}

</script>