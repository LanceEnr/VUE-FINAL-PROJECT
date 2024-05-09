<template>
  <transition name="fade">
    <div v-if="isVisible" class="overlay" @click="handleOverlayClick">
      <div class="modal" @click.stop>
        <div class="video-container">
          <iframe width="960" height="540" :src="youtubeEmbedLink" frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen></iframe>
          <div class="hover-overlay"></div> <!-- Disable interaction overlay specifically for the iframe -->
          <div class="gradient-overlay"></div> <!-- Enhanced Gradient fade effect -->
          <div class="movie-name-overlay">{{ movie.name }} <div class="pricing">
              ₱ {{ thousandFormat(movie.price) }}
            </div>
          </div> <!-- New div for movie name -->
        </div>
        <div class="details">
          <p class="genre">{{ movie.genre }}</p>
          <p class="description">{{ movie.description }}</p>
          <!-- <div class="pricing">
            ₱ {{ thousandFormat(movie.price) }}
          </div> -->
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    youtubeLink: String,
    isVisible: Boolean,
    movie: Object,
    thousandFormat: Function
  },
  computed: {
    youtubeEmbedLink() {
      return `https://www.youtube.com/embed/${this.youtubeLink}?autoplay=1&controls=0&showinfo=0&modestbranding=1&loop=1&playlist=${this.youtubeLink}`;
    }
  },
  methods: {
    handleOverlayClick(event) {
      this.$emit('update:isVisible', false);
    },
    addToCart(movie) {
      this.$emit('add-to-cart', movie);
      this.$emit('update:isVisible', false);
    }
  }
};
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
  backdrop-filter: blur(8px);
}

.modal {
  position: relative;
  width: 960px;
  background-color: rgba(0, 0, 0, 1);
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  z-index: 101;
}

.video-container {
  width: 100%;
  position: relative;
}

.modal iframe {
  border-radius: 8px;
  width: 100%;
  height: 540px;
}

.hover-overlay,
.gradient-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.movie-name-overlay {
  position: absolute;
  bottom: 15px;
  /* Adjust to control distance from bottom */
  left: 30px;
  /* Adjust to control distance from left */
  color: white;
  font-weight: bold;
  /* Text color */
  font-size: 56px;
  /* Text size */
  z-index: 2;
  /* Ensures it's above the iframe and overlays */
}

.hover-overlay {
  background-color: transparent;
}

.gradient-overlay {
  background: linear-gradient(to top, rgba(0, 0, 0, 0.9) 5%, transparent 100%);
  pointer-events: none;
}

.details {
  padding: 0 30px;
  text-align: justify;
  width: 100%;
}


.genre {
  font-size: 20px;
  color: #999;
  margin-bottom: 5px;
}

.description {
  font-size: 16px;
  color: #ccc;
}

.pricing {
  color: red;
  font-size: 30px;

}

.add-to-cart {
  background-color: #e50914;
  border: none;
  color: white;
  padding: 10px 20px;
  margin-left: 20px;
  cursor: pointer;
  border-radius: 5px;
}
</style>
