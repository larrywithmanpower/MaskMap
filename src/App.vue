<template>
  <div id="app">
    <div class="relative overflow-auto min-h-full">
      <a
        href="#"
        class="js-openAsideMenu openAsideMenu bg-blue-500 text-white align-middle pt-1
        px-1 rounded-r-lg shadow hover:bg-red-500 z-20" 
        @click.prevent="openAsideMenu"
      >
        <span class="material-icons text-4xl">
          menu_open
        </span>
      </a>
      <AsideMenu class="absolute top-0 bottom-0" ref="menu" @triggerMarkerPopup="openPopup" />
      <!-- map -->
      <MaskMap ref="map" /> 

      <LightBox class="z-30" />
    </div>
  </div>
</template>

<script>
  import { mapActions } from 'vuex';
  import AsideMenu from '@/components/AsideMenu.vue';
  import LightBox from '@/components/LightBox.vue';
  import MaskMap from '@/components/MaskMap.vue';

  export default {
    components: {
      AsideMenu,
      LightBox,
      MaskMap,
    },
    methods: {
      openAsideMenu() {
        const openBtn = document.querySelector('.js-openAsideMenu');
        const asideMenu = document.querySelector('.js-asideMenu');
        // vue不用在監聽
        openBtn.classList.add('active');
        asideMenu.classList.add('active');
      },
      ...mapActions(['fetchLocations', 'fetchPharmacies']),
      openPopup(id) {
        this.$refs.map.triggerPopup(id);
      }
    },
    mounted() {
      this.fetchLocations();
      this.fetchPharmacies();
    },
  };
</script>

<style lang="scss">
  @import './assets/all';

</style>
