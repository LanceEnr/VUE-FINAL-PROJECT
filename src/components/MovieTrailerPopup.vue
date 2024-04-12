<template>
    <div v-if="isVisible" class="overlay" @click="handleOverlayClick">
      <div class="modal" @click.stop>  <!-- Prevent click event from bubbling up to overlay -->
        <iframe width="560" height="315" :src="youtubeEmbedLink" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
        <button class="close-btn" @click="closePopup">X</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      youtubeLink: String,
      isVisible: Boolean
    },
    computed: {
      youtubeEmbedLink() {
        return `https://www.youtube.com/embed/${this.youtubeLink}`;
      }
    },
    methods: {
      closePopup() {
        this.$emit('update:isVisible', false);
      },
      handleOverlayClick() {
        this.closePopup();  // Close the modal when the overlay is clicked
      }
    }
  };
  </script>
  
  <style scoped>
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
  }
  
  .modal {
    position: relative;
    padding: 20px;
    background: white;
    border-radius: 10px;
  }
  
  .close-btn {
    position: absolute;
    top: 10px;
    right: 10px;
    padding: 5px 10px;
    font-size: 16px;
    cursor: pointer;
    background: none;
    border: none;
    color: #000; /* Ensure the 'X' is visible */
  }
  </style>
  