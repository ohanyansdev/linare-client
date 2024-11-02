<template>
  <div class="slider">
    <div class="slide-container">
      <div v-if="collaborators.length" class="slide-track">
        <div v-for="(collaborator, index) in collaborators" :key="index" class="slide">
          <img :src="collaborator.logo" alt="Partner Logo" class="slide-img" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import CollaboratorsService from "@/services/CollaboratorsService";

export default {
  name: "Partners",
  data() {
    return {
      collaborators: [],
    }
  },

  mounted() {
    this.getCollaborators()
  },

  methods: {
    async getCollaborators() {
      const collaborators = await new CollaboratorsService().get()
      this.collaborators = collaborators.data.collaborators
    }
  }
}
</script>

<style>

.slider {
  max-width: 100% !important;
  width: 100%;
}

.slide-container {
  display: flex;
  overflow: hidden;
}

.slide-track {
  display: flex;
  animation: scroll 10s linear infinite;
}

@keyframes scroll {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(calc(-100% * 3));
  }
}

.slide {
  flex: 0 0 60%;
}

.slide-img {
  width: 140px;
  height: auto;
}
</style>
