<template>
  <transition name="modal" v-if="currStore">
    <div
      class="modal-mask w-full h-full fixed top-0 left-0 z-10 table bg-black bg-opacity-60"
      v-show="showModal"
    >
      <div class="modal-wrapper table-cell align-middle">
        <div
          class="container mx-auto p-3 bg-white shadow rounded"
          @click.self="close()"
        >
          <div class="modal-body">
            <h1 class="text-4xl mb-1 font-bold text-blue-500">
              {{ currStore.name }}
            </h1>
            <hr />
            <h2 class="text-xl my-2 font-bold text-center">營業時間</h2>
            <table class="bg-blue-500 text-black mb-2 w-full">
              <tr class="text-left border-b-2 border-gray-300 text-white">
                <th class="px-4 py-3"></th>
                <th class="px-4 py-3">一</th>
                <th class="px-4 py-3">二</th>
                <th class="px-4 py-3">三</th>
                <th class="px-4 py-3">四</th>
                <th class="px-4 py-3">五</th>
                <th class="px-4 py-3">六</th>
                <th class="px-4 py-3">日</th>
              </tr>

              <tr class="bg-gray-100 border-b border-gray-200">
                <td class="px-4 py-3">早上</td>
                <td
                  class="px-4 py-3"
                  v-for="(s, idx) in servicePeriods[0]"
                  :key="idx"
                >
                  {{ s }}
                </td>
              </tr>
              <!-- each row -->
              <tr class="bg-gray-100 border-b border-gray-200">
                <td class="px-4 py-3">中午</td>
                <td
                  class="px-4 py-3"
                  v-for="(s, idx) in servicePeriods[1]"
                  :key="idx"
                >
                  {{ s }}
                </td>
              </tr>
              <!-- each row -->
              <tr class="bg-gray-100 border-b border-gray-200">
                <td class="px-4 py-3">晚上</td>
                <td
                  class="px-4 py-3"
                  v-for="(s, idx) in servicePeriods[2]"
                  :key="idx"
                >
                  {{ s }}
                </td>
              </tr>
              <!-- each row -->
            </table>

            <ul>
              <li class="mb-1">
                <h2 class="text-blue-500">地址：{{ currStore.address }}</h2>
              </li>
              <li class="mb-1">
                <a :href="`tel:${currStore.phone}`" class="text-blue-500"
                  >電話：{{ currStore.phone }}</a
                >
              </li>
              <li>
                <h2 class="text-blue-500">備註：{{ currStore.note }}</h2>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
  export default {
    name: 'Lightbox',
    computed: {
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
      currStore() {
        return this.$store.state.stores.filter(
          (item) => item.id === this.infoBoxStoreId
        )[0];
      },
      servicePeriods() {
        let servicePeriods = this?.currStore?.['service_periods'] || '';
        servicePeriods = servicePeriods.replace(/N/g, 'O').replace(/Y/g, 'X');

        return servicePeriods
          ? [
              servicePeriods.slice(0, 7).split(''),
              servicePeriods.slice(7, 14).split(''),
              servicePeriods.slice(14, 21).split(''),
            ]
          : servicePeriods;
      },
    },
    methods: {
      close() {
        this.showModal = false;
      },
    },
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .modal-mask {
    transition: opacity 0.3s ease;
  }
</style>

<style>
  .modal-enter {
    opacity: 0;
  }
  .modal-leave-active {
    opacity: 0;
  }
  .modal-enter .modal-container,
  .modal-leave-active .modal-container {
    transform: scale(1.1);
  }
</style>
