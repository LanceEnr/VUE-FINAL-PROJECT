<script>
import { watch, ref } from 'vue';
import {
  isTouchDevice,
  isSmallSize,
  cart,
  isCartOpen,
  getCoverStyle,
  removeFromCart,
  thousandFormat,
} from '@/services/index';

export default {
  props: {
    totalPrice: {
      type: Number,
      default: 0,
    },
  },
  setup(props) {
    const total = ref(props.totalPrice);

    watch(() => props.totalPrice, animateTotal);

    function animateTotal() {
      const diff = ~~((props.totalPrice - total.value) * 0.15);
      if (diff === 0) return (total.value = props.totalPrice);
      total.value += diff;
      window.requestAnimationFrame(animateTotal);
      // setTimeout(() => animateTotal());
    }

    return {
      total,
      isTouchDevice,
      isSmallSize,
      cart,
      isCartOpen,
      getCoverStyle,
      removeFromCart,
      thousandFormat
    };
  },
};
</script>

<template>
  <section :class="['cart-page', { cartOpen: isCartOpen }]">
    <h3 class="message" v-if="!cart.data.length">
      You haven't added any movies to your shopping cart yet :(
    </h3>
    <section v-if="cart.data.length" :class="['panel', { hoverInteraction: !isTouchDevice }]">
      <h2>Movie Shopping Cart</h2>
      <ul>
        <li v-for="(movie, idx) in cart.data" :key="movie.name">
          <div class="remove" @click="removeFromCart(idx)">✕</div>
          <div class="thumbnail" :style="getCoverStyle(movie.cover)"></div>
          <h3 class="name">
            {{ movie.name }}
            {{
    isSmallSize ? '' : `（${movie.genre.replaceAll('、', ' / ')}）`
  }}
          </h3>
          <h3 class="price">₱ {{ thousandFormat(movie.price) }}</h3>
        </li>
      </ul>
      <hr v-if="cart.data.length" />
      <h2 v-if="cart.data.length" class="totaTotall-price">
        Total ₱ {{ thousandFormat(total) }}
      </h2>
    </section>
  </section>
</template>

<style lang="sass" scoped>
@import '@/assets/style.sass'

.cart-page
  width: 100%
  height: 100%
  position: fixed
  left: 50%
  top: 50%
  transform: translate(-50%, -50%)
  display: flex
  justify-content: center
  align-items: center
  color: $lightGrey
  // background-image: linear-gradient(10deg, $black 0%, $black 50%, transparent 100%)
  background-color: rgba($black, 0.9)
  opacity: 0
  pointer-events: none
  transition: opacity $transitionTime
  &.cartOpen
    opacity: 1
    pointer-events: initial
    z-index: 150

.message
  color: $mediumGrey
  letter-spacing: 0.5px
  @media screen and (min-width: 1281px)
    transform: scale(1.1)

.panel
  width: 65%
  max-width: 1100px
  &.hoverInteraction
    .remove
      opacity: 0
    li:hover
      background-color: rgba(white, 0.1)
      transform: translateY(-5px)
      .remove
        opacity: 1
        cursor: pointer
  @media screen and (max-width: 767px) // < 678
    width: 90%
  @media screen and (min-width: 1281px) // > 1280
    transform: scale(1.1)

h2
  margin-bottom: 30px

ul
  // Scrollable window
  max-height: 400px
  overflow: hidden auto
  @media screen and (max-height: 449px) // height < 450
    max-height: 190px
  @media screen and (min-height: 767px) // height > 768
    max-height: 490px

li
  padding: 15px 0
  padding-right: 25px
  display: flex
  align-items: center
  opacity: 0.8
  border-radius: 5px
  transition: $transitionTime
  // &:hover
  //   background-color: rgba(white, 0.1)
  //   transform: translateY(-5px)
  //   .remove
  //     opacity: 1
  //     cursor: pointer

.remove
  margin: 0 10px
  display: flex
  justify-content: center
  align-items: center
  width: 25px
  height: 70px
  transition: $transitionTime

.thumbnail
  margin-right: 20px
  width: 50px
  height: 70px

.name
  font-size: 17px
  font-weight: normal

.price
  margin-left: auto

hr
  margin: 20px 0

.total-price
  margin: 0
  padding-right: 25px
  text-align: right
</style>
