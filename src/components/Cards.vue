<script>
import MovieTrailerPopup from './MovieTrailerPopup.vue';
import { ref, watch, onMounted } from 'vue';
import {
  isTouchDevice,
  movies,
  currentMovieCover,
  isCartOpen,
  getCoverStyle,
  horizontalScroll,
  addToCart,
  truncate,
  thousandFormat,
} from '@/services/index';

export default {
  components: {
    MovieTrailerPopup
  },
  setup() {
    const cardsElement = ref(null);
    const isTrailerVisible = ref(false);
    const currentTrailer = ref('');
    const currentMovie = ref(null);  // Initialize the currentMovie reference

    // Watcher to log visibility changes
    watch(isTrailerVisible, (newValue) => {
      console.log('Visibility changed:', newValue);
    });

    onMounted(() => {
      const cardsEl = cardsElement.value;
      cardsEl.classList.add('demo');
      setTimeout(() => cardsEl.classList.remove('demo'), 2500);
    });

    const showTrailer = (movie) => {
      currentTrailer.value = movie.trailer; // Set the trailer URL
      currentMovie.value = movie; // Set the current movie object
      isTrailerVisible.value = true;
    };

    return {
      cardsElement,
      isTrailerVisible,
      currentTrailer,
      currentMovie, // Make sure to return this so it's usable in the template
      isTouchDevice,
      movies,
      currentMovieCover,
      isCartOpen,
      getCoverStyle,
      horizontalScroll,
      addToCart,
      truncate,
      thousandFormat,
      showTrailer
    };
  },
};
</script>

<template>
  <section class="movie" @wheel.prevent="horizontalScroll($event)">
    <section :class="['cards', { cartOpen: isCartOpen }]" ref="cardsElement">
      <article :class="['card', { hoverInteraction: !isTouchDevice }]" v-for="(movie, idx) in movies.data"
        :key="movie.name" @click="showTrailer(movie)">
        <div class="card-left">
          <div class="cover" :style="getCoverStyle(movie.cover)"></div>
        </div>
        <div class="card-right">
          <h2 class="name">{{ movie.name }}</h2>
          <h4 class="genre">{{ movie.genre }}</h4>
          <p class="description">{{ truncate(movie.description, 95) }}</p>
          <div class="price">₱ {{ thousandFormat(movie.price) }}</div>
          <button :class="['add', { inCart: movie.isInCart }]" @click.stop="addToCart(movie, idx, $event)">
            {{ movie.isInCart ? 'Already in Cart' : 'Add to Cart' }}
          </button>
        </div>
      </article>
    </section>
  </section>
  <MovieTrailerPopup
    :youtubeLink="currentTrailer"
    :isVisible="isTrailerVisible"
    :movie="currentMovie"
    :thousandFormat="thousandFormat"  
    @update:isVisible="value => isTrailerVisible = value"
  />

  <div v-show="currentMovieCover" class="moving-cover" :style="getCoverStyle(currentMovieCover)"></div>
</template>

<style lang="sass" scoped>
@import '@/assets/style.sass'

.movie
  height: 100%

.cards
  padding: 0 20%
  height: 100%
  display: flex
  align-items: center
  transition: $transitionTime, left 0s
  position: relative // Change left to horizontally scroll
  &.demo
    animation: demoScroll 2.5s ease-out
  &.cartOpen
    filter: blur(10px)

.card
  margin: 0 80px
  padding: 20px 20px
  cursor: pointer
  width: 484px
  display: inline-flex
  flex-shrink: 0
  background-color: rgba(white, 0.9)
  border-radius: 5px
  transform: translateY(15%)
  transition: $transitionTime
  &.hoverInteraction
    &:hover
      transform: translateY(5px)
      .cover
        transform: translateY(-15px)
    button
      &:hover
        color: white
        background-color: $orange
      &.inCart
        background-color: $grey
        cursor: default
  @media screen and (max-width: 767px) and (orientation: landscape) // < 768
    margin: 0
    transform: translateY(15%) scale(0.8)
    &.hoverInteraction:hover
      transform: translateY(5px) scale(0.8)
  @media screen and (min-width: 1281px) // > 1280
    margin: 0 110px
    transform: translateY(15%) scale(1.1)
    &.hoverInteraction:hover
      transform: translateY(5px) scale(1.1)

.card-left
  // flex: 1
  .cover
    margin-top: -50px
    width: 198px
    height: 264px
    border-radius: 5px
    box-shadow: $coverShadow
    transition: $transitionTime

.card-right
  // flex: 2
  margin-left: 10px
  padding: 10px
  .name
    color: $darkGrey
  .genre
    margin: 14px 0
    font-weight: normal
    opacity: 0.8
  .description
    margin: 10px 0
    font-size: 13px
    letter-spacing: 0.2px
    line-height: 1.3
    color: $grey
    opacity: 0.8
  .price
    display: inline-block
    font-weight: 500
  button
    margin-left: 23px
    padding: 7px 10px
    font-size: 13px
    color: rgba(white, 0.9)
    background-color: $mediumGrey
    border: none
    border-radius: 50px
    cursor: pointer
    transition: background-color 0.8s
    // &:hover // .card.hoverInteraction button:hover
    &.inCart
      background-color: $grey
      cursor: default

.moving-cover
  position: fixed
  right: 50px
  top: 50px
  width: 50px
  height: 70px
  opacity: 0
</style>
