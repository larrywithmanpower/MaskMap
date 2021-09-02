<template>
  <div
    class="w-full md:w-6/12 lg:w-5/12 xl:w-4/12 shadow font-sans asideMenu js-asideMenu overflow-hidden md:pb-60 lg:pb-48"
  >
    <div class="container pt-2 pb-3 px-3 bg-blue-500 sticky top-0">
      <!-- close btn -->
      <a
        href="#"
        class="text-white hover:text-red-500"
        @click.prevent="closeAsideMenu"
      >
        <span class="material-icons">
          close
        </span>
      </a>
      <!-- select -->
      <ul>
        <li class="ssm:flex justify-between items-center mb-3 relative">
          <!-- arrow -->
          <span class="material-icons absolute right-4 top-11 md:top-3.5">
            arrow_drop_down
          </span>
          <!-- city -->
          <label for="city" class="text-white cursor-pointer block mb-2"
            >縣市：</label
          >
          <select
            class="rounded-full py-3 px-6 w-full ssm:w-5/6 placeholder-gray-500 border border-transparent
            focus:outline-none focus:ring focus:ring-red-500 focus:ring-opacity-70 focus:border-transparent cursor-pointer"
            id="city"
            v-model="currCity"
          >
            <option v-for="city in cityList" :key="city">{{ city }}</option>
          </select>
        </li>
        <li class="ssm:flex justify-between items-center mb-3 relative">
          <!-- arrow -->
          <span class="material-icons absolute right-4 top-11 md:top-3.5">
            arrow_drop_down
          </span>
          <!-- distriction -->
          <label for="district" class="text-white cursor-pointer block mb-2"
            >行政區：</label
          >
          <select
            class="rounded-full py-3 px-6 w-full ssm:w-5/6 placeholder-gray-500 border border-transparent
            focus:outline-none focus:ring focus:ring-red-500 focus:ring-opacity-70 focus:border-transparent cursor-pointer"
            id="district"
            v-model="currDistrict"
          >
            <option v-for="district in districtList" :key="district.id">{{
              district.name
            }}</option>
          </select>
        </li>
        <li class="ssm:flex justify-between items-center relative">
          <!-- keyword -->
          <label for="keywords" class="text-white cursor-pointer block mb-2"
            >關鍵字：</label
          >
          <input
            class="rounded-full py-3 px-6 w-full ssm:w-5/6 placeholder-gray-500 border border-transparent
            focus:outline-none focus:ring focus:ring-red-500 focus:ring-opacity-70 focus:border-transparent cursor-pointer"
            id="keywords"
            placeholder="請輸入關鍵字"
            v-model="keywords"
          />
          <a
            href="#"
            class="absolute right-4 top-12 ssm:top-3 text-black hover:text-red-500"
            @click.prevent="cleanKeywords()"
          >
            <span class="material-icons">
              close
            </span>
          </a>
        </li>
      </ul>
    </div>
    <!-- stores -->
    <ul
      class="container bg-white pt-2 pb-2 px-3 overflow-auto h-full scrollNone"
    >
      <li class="mb-3 pb-3 border-b cursor-pointer" v-for="s in filteredStores" :key="s.id" @click="emitTrigger(s.id)">
        <h1
          class="text-xl font-bold mb-1"
          v-html="keywordHighlight(s.name)"
        ></h1>
        <div class="flex items-center justify-between">
          <ul class="text-xs text-gray-500 mb-2 w-9/12">
            <li class="mb-1.5">
              <i class="fas fa-head-side-mask mr-2"></i>
              成人口罩：
              {{ s.mask_adult }} 片
            </li>
            <li class="mb-1.5">
              <i class="fas fa-baby mr-2.5"></i>
              孩童口罩：
              {{ s.mask_child }}
            </li>
            <li class="mb-1.5">{{ s.note }}</li>
            <li>更新時間：{{ s.updated }}</li>
          </ul>
          <button
            class="p-3 border text-center hover:shadow rounded"
            @click="openInfoBox(s.id)"
          >
            <span class="material-icons"> info </span><br />
            詳細資訊
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex';

  export default {
    watch: {
      districtList(renew) {
        const [arr] = renew;
        this.currDistrict = arr.name;
      },
    },
    computed: {
      currCity: {
        get() {
          return this.$store.state.currCity;
        },
        set(value) {
          this.$store.commit('setcurrCity', value);
        },
      },
      currDistrict: {
        get() {
          return this.$store.state.currDistrict;
        },
        set(value) {
          this.$store.commit('setcurrDistrict', value);
        },
      },
      keywords: {
        get() {
          return this.$store.state.keywords;
        },
        set(value) {
          this.$store.commit('setKeywords', value);
        },
      },
      showModal: {
        get() {
          return this.$store.state.showModal;
        },
        set(value) {
          this.$store.commit('setshowModal', value);
        },
      },
      infoBoxStoreId: {
        get() {
          return this.$store.state.infoBoxStoreId;
        },
        set(value) {
          this.$store.commit('setInfoBoxStoreId', value);
        },
      },
      ...mapGetters(['cityList', 'districtList', 'filteredStores']),
    },
    methods: {
      closeAsideMenu() {
        const openBtn = document.querySelector('.js-openAsideMenu');
        const asideMenu = document.querySelector('.js-asideMenu');
        // vue不用在監聽
        openBtn.classList.remove('active');
        asideMenu.classList.remove('active');
      },
      // highlight
      keywordHighlight(name) {
        return name.replace(
          new RegExp(this.keywords, 'g'),
          `<span class="text-red-500">${this.keywords}</span>`
        );
      },
      openInfoBox(storeId) {
        this.showModal = true;
        this.infoBoxStoreId = storeId;
      },
      cleanKeywords() {
        this.keywords = '';
      },
      emitTrigger(id) {
        this.$emit('triggerMarkerPopup', id);
        this.closeAsideMenu();
      }
    },
  };
</script>
