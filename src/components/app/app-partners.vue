<template>

  <div v-if="collaborators.length" class="partners-block mb-2">
    <div class="title">{{ $t('about.our_collaborators') }}</div>

    <div class="partners-block-inner">
      <div class="partners_part">
        <div v-for="(collaborator, index) in collaborators" :key="index" class="partner-block">
          <div class="m-1">
            <img class="partner_img" :src="collaborator.logo" alt="Partner Logo"/>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CollaboratorsService from "../../services/CollaboratorsService";

export default {
  name: 'app-partners',
  data() {
    return {
      collaborators: []
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

<style scoped lang="scss">

@mixin white-gradient {
  background: linear-gradient(to right,  rgba(255,255,255,1) 0%,rgba(255,255,255,0) 100%);
}

.partners-block {
  background-color: #14457B;
  padding: 10px 20px

}
.partners_part {
  display: flex;
  justify-content: start;
  gap: 100px;
  align-items: center;
  flex-wrap: wrap;
}

$animationSpeed: 40s;

// Animation
@keyframes scroll {
  0% { transform: translateX(0); }
  100% { transform: translateX(calc(-250px * 7))}
}


// Styling
.partners-block-inner {
  margin: auto;
  overflow:hidden;
  position: relative;

  .partners_part {
    animation: scroll $animationSpeed linear infinite;
    width: calc(250px * 14);
  }

  .partner-block {
    text-align: center;
    height: 150px;
    width: 200px;
    display: flex;
    justify-content: center;
    flex-direction: column;
  }
}

.title {
  color: #FFFFFF;
  font-size: 30px;
  text-align: center;
  font-weight: 500;
  margin-bottom: 10px;
}

.partner_img {
  width: 200px;
  aspect-ratio: 3/2;
  object-fit: contain;
  mix-blend-mode: color-burn;
}

@media only screen and (max-width: 1065px) {
  .partner_img {
    width: 150px;
  }
}

@media only screen and (max-width: 320px) {
  .partner_img {
    width: 130px;
  }
}

</style>