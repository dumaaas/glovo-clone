<template>
  <div class="bg-white py-[60px] xl:px-[25.5%] px-[7.5%]">
    <h1 v-if="locationData" class="text-black text-[28px] leading-[34px] font-medium">
      <span
        class="
          bg-[#FFC244]
          rounded-[100px]
          pl-[3px]
          pr-[10px]
          text-[17px]
          font-normal
          leading-[1.2]
        "
        ><mark
          class="
            text-[28px]
            leading-[34px]
            text-black
            font-medium
            bg-transparent
            text-left
          "
          >Hrana</mark
        ></span
      >u blizini
    </h1>
     <h1 v-else class="text-black text-[28px] leading-[34px] font-medium">
      <span
        class="
          bg-[#FFC244]
          rounded-[100px]
          pl-[3px]
          pr-[10px]
          text-[17px]
          font-normal
          leading-[1.2]
        "
        ><mark
          class="
            text-[28px]
            leading-[34px]
            text-black
            font-medium
            bg-transparent
            text-left
          "
          >Odaberite</mark
        ></span
      >adresu kako bi pronašli hranu u blizini
    </h1>
    <div
      class="flex flex-wrap items-center justify-between gap-[28px] pt-[42px]" v-if="locationData"
    >
      <div
        v-for="d in data"
        :key="d.name"
        class="
          md:flex-[30%]
          flex-[100%]
          rounded-[10px]
          shadow-lg
          overflow-hidden
          cursor-pointer
        "
      >
        <div
          class="
            overflow-y-hidden
            relative
            w-full
            h-[168px]
            bg-no-repeat bg-cover
            rounded-tl-[10px] rounded-tr-[10px]
            bg-center
            hover:scale-105
            transition-all
            duration-300
            ease-in-out
          "
          style="
            background-image: url('https://res.cloudinary.com/glovoapp/w_450,h_250,c_fill,f_auto,q_30/Stores/wk3ncvypox5aizmkko6i');
          "
        >
          <p
            class="
              w-full
              font-medium
              text-center
              absolute
              top-[50%]
              left-[50%]
              translate-x-[-50%] translate-y-[-50%]
              text-[28px]
              leading-[34px]
              text-white
              px-[8px]
            "
          >
            {{ d.poi.name }}
          </p>
        </div>
        <div class="flex justify-between items-center px-[8px] h-[45px] overflow-hidden">
          <p class="text-[14px] leading-[22px] text-[#6e6e6e] font-medium">
            {{
              d.address.freeformAddress.length > 17
                ? d.address.freeformAddress.substring(0, 17) + "..."
                : d.address.freeformAddress
            }}
          </p>
          <p class="text-[14px] leading-[22px] text-[#6e6e6e] font-medium">
            {{ d.poi.phone }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    locationData: {
      type: Object,
      default: {},
    },
  },
  data() {
    return {
      apiKey: "RqIZRb0WFPFyGUMAe5L84jiUfjH8Txb7",
      data: null,
    };
  },
  watch: {
    locationData: {
      handler() {
        console.log('kako brate?')
        this.getNearbyPlaces();
      },
      deep: true,
    },
  },
  async mounted() {},
  methods: {
    async getNearbyPlaces(query = "", limit = 9, radius = 10000) {
      console.log('mada si mi bila draga?')
      let baseUrl = "https://api.tomtom.com/search/2/poiSearch";
      let queryString = `limit=${limit}&lat=${this.locationData.long}&lon=${this.locationData.lat}&radius=${radius}&key=${this.apiKey}&categorySet=7315`;
      await this.$axios
        .$get(`${baseUrl}/${query}.json?${queryString}`)
        .then((response) => {
          this.data = response.results;
          console.log(this.data, "datatata");
        });
    },
  },
};
</script>

<style lang="scss" scoped>
</style>