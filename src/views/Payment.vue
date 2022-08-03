<template>
    <div class="payment">
        <div class="payment--content">
            <router-link to="/" class="payment--go-back">← Voltar</router-link>
            <Order />
        </div>
        <Cart class="cart-menu" />
    </div>
</template>

<script>
import Cart from '@/components/Cart.vue';
import Mixin from '@/mixins/mixins';
import Order from '@/components/Order.vue';

export default {
    name: 'Payment',
    components: {
        Cart,
        Order
    },
    mixins: [Mixin],
}
</script>

<style lang="less" scoped>
.payment {
    display: flex;

    &--content {
        flex-grow: 1;
        padding: 50px;

        .payment--go-back {
            font-weight: 600;
            font-size: 16px;
            text-decoration: none;
            color: black;
        }
    }

    @media @small-desktops {
        &--content {
            padding: 50px 20px;
        }

        .cart-menu {
            display: none;
        }
    }

}
</style>