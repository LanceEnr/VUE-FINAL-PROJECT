<template>
  <transition name="fade">
    <div v-if="isVisible" class="overlay" @click="handleOverlayClick">
      <div class="modal" @click.stop> <!-- Stops click event from propagating to overlay -->
        <iframe width="800" height="450" :src="youtubeEmbedLink" frameborder="0"
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen></iframe>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    youtubeLink: String,
    isVisible: Boolean
  },
  computed: {
    youtubeEmbedLink() {
      // Compute the full YouTube embed URL from the provided video ID
      return `https://www.youtube.com/embed/${this.youtubeLink}`;
    }
  },
  methods: {
    handleOverlayClick(event) {
      // Close the modal by emitting an event to the parent component
      console.log('Overlay clicked');

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
  backdrop-filter: blur(8px);
}

.modal {
  position: relative;
  width: 800px;
  height: 450px;
  overflow: hidden;
  /* Ensures the iframe's border radius is visible */
}

.modal iframe {
  border-radius: 20px;
  /* Adjust this value to your preference */
}
</style>
