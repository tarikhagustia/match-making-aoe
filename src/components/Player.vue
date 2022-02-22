<template>
  <div class="d-flex card rounded mb-3 text-gold" v-bind:class="{ 'glow-border': (elo >= 4) }">
      <div class=""><img width="70" :src="profile" :alt="name"></div>
      <div class="mx-2">
          <p class="m-0 name">{{ name }}</p>
          <p class="m-0 text-xs">Elo {{ elo }}</p>
      </div>
  </div>
</template>
<style scoped>
.name {
    font-size: 1.2rem;
}
</style>
<script>


export default {
  name: "HelloWorld",
  props: ['name', 'elo'],
  data() {
    return {
      
    };
  },
  mounted() {
   
  },
  computed: {
      profile() {
          if (this.elo >= 4) {
              return "https://static.wikia.nocookie.net/ageofempires/images/2/28/Paladin_aoe2DE.png";
          }

          if(this.elo >= 3) {
              return "https://static.wikia.nocookie.net/ageofempires/images/5/54/Champion_aoe2DE.png";
          }

          if(this.elo >= 2) {
              return "https://static.wikia.nocookie.net/ageofempires/images/d/dc/Arbalester_aoe2DE.png";
          }

          return "https://static.wikia.nocookie.net/ageofempires/images/6/68/MaleVillDE.jpg";
          
      }
  }
};
</script>