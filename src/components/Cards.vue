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
    const currentMovie = ref(null);
    const hoverCover = ref('');

    const showTrailer = (movie) => {
      currentTrailer.value = movie.trailer;
      currentMovie.value = movie;
      isTrailerVisible.value = true;
    };

    const handleMouseEnter = (movie) => {
      hoverCover.value = movie.hoverCoverUrl;
    };

    const handleMouseLeave = () => {
      hoverCover.value = '';
    };

    return {
      cardsElement,
      isTrailerVisible,
      currentTrailer,
      currentMovie,
      hoverCover,
      isTouchDevice,
      movies,
      currentMovieCover,
      isCartOpen,
      getCoverStyle,
      horizontalScroll,
      addToCart,
      truncate,
      thousandFormat,
      showTrailer,
      handleMouseEnter,
      handleMouseLeave
    };
  },
};
</script>

<template>
  <section class="movie-container">
    <div class="background" :style="{
      backgroundImage: `url(${hoverCover})`,
      backgroundSize: 'cover',
      filter: hoverCover ? 'blur(6px) brightness(50%)' : 'none'
    }"></div>

    <section class="movie" @wheel.prevent="horizontalScroll($event)">
      <section :class="['cards', { cartOpen: isCartOpen }]" ref="cardsElement">
        <article :class="['card', { hoverInteraction: !isTouchDevice }]" v-for="(movie, idx) in movies.data"
          :key="movie.name" @click="showTrailer(movie)" @mouseenter="handleMouseEnter(movie)"
          @mouseleave="handleMouseLeave">
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
    <MovieTrailerPopup :youtubeLink="currentTrailer" :isVisible="isTrailerVisible" :movie="currentMovie"
      :thousandFormat="thousandFormat" @update:isVisible="value => isTrailerVisible = value" />
  </section>
</template>

<style lang="sass" scoped>
@import '@/assets/style.sass'

.movie-container
  width: 100vw
  height: 100vh
  position: relative

.background
  position: absolute
  width: 100%
  height: 100%
  background-size: cover
  background-position: center
  background-repeat: no-repeat
  transition: background-image 0.7s ease-in-out, background-size 0.7s ease-in-out, filter 0.5s ease-in-out, opacity 0.7s ease-in-out

.movie
  width: 100%
  height: 100%
  position: relative
  z-index: 2  // Increased z-index to make sure it's above background but below the popup

.cards
  padding: 0 20%
  height: 100%
  display: flex
  align-items: center
  transition: $transitionTime, left 0s
  position: relative

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
  transition: transform 0.5s ease, box-shadow 0.5s ease, filter 0.5s ease
  &.hoverInteraction
    &:hover
      transform: translateY(5px) scale(1.05)
      box-shadow: 0 8px 16px rgba(0,0,0,0.2)
      .cover
        transform: scale(1.1)
        filter: brightness(1.1) contrast(1.2)
    button
      &:hover
        color: white
        background-color: $orange
      &.inCart
        background-color: $grey
        cursor: default


.movie-trailer-popup
  position: fixed
  z-index: 100 // High z-index to ensure it appears on top

.card-left
  .cover
    margin-top: -50px
    width: 198px
    height: 264px
    border-radius: 5px
    box-shadow: $coverShadow
    transition: $transitionTime

.card-right
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

.moving-cover
  position: fixed
  right: 50px
  top: 50px
  width: 50px
  height: 70px
  opacity: 0
</style>
