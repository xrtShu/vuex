<template>
    <div class="cart">
        <h2>My Cart</h2>
        <p v-show="!products.length">
            <i>购物车空空如也</i>
        </p>
        <ul>
            <li v-for="p in products">
                {{ p.title }} - {{ p.price | currency }} x {{ p.quality }}
            </li>
        </ul>
        <p>总计：{{ total | currency }}</p>
        <p>
            <button :disabled="!products.length" @click="checkout(products)">结算</button>
        </p>
        <p v-show="checkoutStatus">Checkout {{ checkoutStatus }}.</p>
    </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
    computed: {
        ...mapGetters({
            products: 'cartProducts', // 这里调用了公共的getters, 详情见getters 解释
            checkoutStatus: 'checkoutStatus' // 在cart 模块中的getters 读取
        }),
        total () {
            console.log('Hi total!') // 页面渲染完成，直接调用
            return this.products.reduce((total, p) => {
                return total + p.price * p.quality
            }, 0)
        }
    },
    methods: {
        checkout (products) {
            this.$store.dispatch('checkout', products)
            // 派发'checkout' 触发cart 模块中的actions, 提交异步请求
        }
    }
}
</script>