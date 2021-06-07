<template>
  <div class="cart">
    <div class="cart-header">
      <h3>Your Cart</h3>
      <Breadcrumb :breadcrumbs="breadcrumbs"/>
    </div>

    <CartTable :cartItems="cartItems" @removeItem="removeItem" @updateQuantity="updateQuantity" />
    <CartFoot :subtotal="subtotal" :tax="tax" :total="total" @checkout="checkout"/>
  </div>
</template>

<script>
import Breadcrumb from '@/components/cart/Breadcrumb';
import CartTable from '../components/cart/CartTable';
import CartFoot from '../components/cart/CartFoot';
import {mapGetters, mapActions} from 'vuex';
export default {
  name: 'Cart',
  components: {
    Breadcrumb,
    CartTable,
    CartFoot
  },
  data() {
    return {
      breadcrumbs: [
        {
          title: 'Home',
          link: '/',
        },
        {
          title: 'Checkout',
          link: '#',
        },

      ],
    };
  },
  methods: {
    ...mapActions([
      'UPDATE_QUANTITY',
        'REMOVE_ITEM_FROM_CART',
        'CHECKOUT'
    ]),
    updateQuantity(data){
      this.UPDATE_QUANTITY(data)
    },
    removeItem(id){
      this.REMOVE_ITEM_FROM_CART(id)

    },
    checkout(){
      this.CHECKOUT({
        items: this.cartItems,
        pricing: {
          subtotal: this.subtotal,
          tax: this.tax,
          total: this.total,
        }
      }).then(res=>{
        alert(res)
      })
    }
  },
  computed: {
    ...mapGetters({
      cartItems: 'getCart'
    }),

    subtotal(){
      let subtotal = 0
      this.cartItems.forEach(item =>{
       subtotal +=( item.price*item.quantity)
      })
      return subtotal
    },
    tax(){

      return 0
    },
    total(){
     return this.subtotal-this.tax
    }
  }
};
</script>

<style lang="scss" scoped>
.cart {
  padding: 72px 46px;
  background: #1B1B1B;
  min-height: calc(100vh - 70px);

  .cart-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    h3 {
      font-size: 28px;
      line-height: 100%;
      color: #FFFFFF;
      font-weight: 400;
      margin-bottom: 0;
    }
  }
}
@media (max-width: 991px) {
  .cart{
    padding: 62px 15px
  }
}
</style>
