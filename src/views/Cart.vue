<template>
<div class="conteiner">
    <template v-if="cartItems.length > 0">
    <ul class="list-group">
        <li class="list-group-item" v-for="(item, key) in cartItems" :key="key">
            {{ item.title }} - {{ item.price }}
        </li>
    </ul>
    <div class="panel">
        Quality:{{ cartCount }}
        Total: {{cartTotalPrice}}
    </div>

    <button class="btn btn-seccess" @click="handleGetPaymentIntent">Купить</button>
    <template v-if="paymentIntent">
    <Card
          ref="card"
          class="stripe-card"
          :class="{ complete }"
          stripe="pk_test_51JyMN5Gf6Ylu9XbRGZy5TsEi4or364LIE5EHHnuNnmTCaqLVA9XwWJy1El29eSeUc2rAN0ATU6EFYDePuuQy5jKc00fRTaHIDu"
          :options="stripeOptions"
          @change="complete = $event.complete"
        />
    <button class='btn btn-seccess' @click='pay' :disabled='!complete'>Pay with credit card</button>
    </template>
    </template>
    <template v-else>
        Your cart is empty please add items to it
    </template>
</div>

</template>

<script>
import { mapGetters, mapActions, mapMutations } from "vuex"
import { Card, handleCardPayment } from 'vue-stripe-elements-plus'
export default {
    name:"Cart",
    components:{
        Card
    },
    data:() => ({
        complete: false,
        stripeOptions: {
      // see https://stripe.com/docs/stripe.js#element-options for details
        },
        paymentIntent: null
    }),
    computed:{
        ...mapGetters({
            cartItems: 'cartItems',
            cartTotalPrice:'cartTotalPrice',
            cartCount:'cartCount'
        })
    },
    methods:{
        ...mapMutations({
            clearCart:'clearCart'
        }),
        ...mapActions({
            handleBuy: "handleBuy"
        }),
        async handleGetPaymentIntent(){
             this.paymentIntent = await this.handleBuy()
        },
        async pay(){
            try {
            await handleCardPayment(this.paymentIntent)
            this.clearCart()
            } catch (err) {
            console.log(err)
            }
        }
    }
}
</script>

<style scoped>
    .stripe-card{
        width: 300px;
        border: 1px solid grey
    }
    .stripe-card-complete{
        border-color: green
    }
</style>