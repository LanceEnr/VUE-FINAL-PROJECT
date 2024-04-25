<template>
  <transition name="fade">
    <div v-if="isVisible" class="overlay" @click="handleOverlayClick">
      <div class="modal" @click.stop>
        <div class="content">
          <iframe :src="youtubeEmbedLink" frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen style="width: 100%; height: 100%;"></iframe>
          <div class="movie-details">
            <h2>{{ movie.name }}</h2>
            <p>{{ movie.description }}</p>
            <h4>Genre: {{ movie.genre }}</h4>
            <h4>Price: ₱ {{ thousandFormat(movie.price) }}</h4>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import { thousandFormat } from '@/services/index';

export default {
  props: {
    youtubeLink: String,
    isVisible: Boolean,
    movie: Object
  },
  computed: {
    youtubeEmbedLink() {
      return `https://www.youtube.com/embed/${this.youtubeLink}`;
    }
  },
  methods: {
    handleOverlayClick(event) {
      this.$emit('update:isVisible', false);
    }
  },
  setup() {
    return {
      thousandFormat
    };
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
  padding: 20px; /* Adds padding around the modal */
}

.modal {
  width: 80%; /* Adjust based on your design preference */
  max-width: 1200px; /* or any other max-width */
  display: flex;
  flex-direction: row;
  height: auto; /* Adjust this to 'auto' or a specific value like '600px' */
}


.content {
  display: flex;
  width: 100%; /* Ensures the content uses all the space of .modal */
  background-color: #333131;
  border-radius: 8px;
  overflow: hidden;
}

iframe {
  flex-grow: 2; /* Adjusts how much space the iframe should take relative to details */
  min-height: 450px; /* Set a minimum height for better visibility */
}

.modal iframe {
  border-radius: 20px;
  /* Adjust this value to your preference */
}

.movie-details {
  flex-basis: 300px; /* Fixed width for details */
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-color: #f4f4f4; /* Light grey background to enhance readability */
  overflow: auto; /* Allows scrolling if the content is taller than the container */
  color: #333; /* Darker text for better contrast */
  font-size: 16px; /* Larger font size for better readability */
  line-height: 1.5; /* Increased line height for clearer separation of lines */
  font-family: 'Roboto', sans-serif;
}

.movie-details h2, h4, p {
  margin-bottom: 10px; /* Adds space between paragraphs and headings */
}

</style>
