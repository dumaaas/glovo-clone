<template>
  <div>
    <div class="px-[7.5%]">
      <div
        v-if="!locationData"
        @click="showModal = true"
        class="flex items-center justify-center flex-col pb-[25px] gap-[4px]"
      >
        <p class="text-[18px] leading-[25px] text-black font-bold text-center">
          Unesite svoju adresu i saznaj
          <span
            class="
              bg-white
              rounded-[100px]
              px-[3px]
              text-[6px]
              font-normal
              leading-[1.4]
            "
            ><mark
              class="
                text-[18px]
                leading-[25px]
                text-black
                font-medium
                bg-transparent
              "
              >šta je blizu tebe</mark
            ></span
          >
        </p>
        <div
          class="
            h-[50px]
            text-[16px]
            leading-[1.4]
            font-light
            w-full
            max-w-[540px]
            bg-white
            rounded-[8px]
            flex
            gap-[14px]
            text-[#c1c1c1]
          "
        >
          <div
            class="
              bg-[#00A082]
              w-[50px]
              h-full
              rounded-tl-[8px] rounded-bl-[8px]
              flex
              items-center
              justify-center
            "
          >
            <img
              src="https://res.cloudinary.com/glovoapp/image/fetch///https://glovoapp.com/images/icons/flag--white.svg"
              role="presentation"
              alt=""
              data-v-6ea6e892=""
            />
          </div>
          <div class="flex items-center">
            <span> Koja je tvoja adresa? </span>
          </div>
        </div>
      </div>
      <div v-else class="md:pt-0 pt-[20px]">
        <h2
          class="text-[24px] leading-[29px] text-black font-light text-center"
        >
          Dostava na adresu
          <span
            class="
              flex flex-row
              items-center
              justify-center
              gap-[3px]
              cursor-pointer
            "
            @click="showModal = true"
          >
            <span class="font-bold"
              >{{ locationData.street }} {{ locationData.houseNumber }}</span
            >
            <img
              data-v-393121f2=""
              src="https://res.cloudinary.com/glovoapp/image/fetch//q_auto/https://glovoapp.com/images/landing/dropdown-black.svg"
              role="presentation"
              alt=""
              class="address-picker__arrow mt-[5px]"
            />
          </span>
        </h2>

        <div
          class="
            flex
            items-center
            justify-center
            gap-[16px]
            flex-wrap
            md:py-[100px]
            py-[60px]
          "
        >
          <div
            class="
              flex flex-col
              items-center
              justify-center
              gap-[5px]
              rounded-full
              h-[114px]
              w-[114px]
              bg-white
              cursor-pointer
              hover:scale-110
              transform
              transition-all
              duration-150
              ease-in-out
            "
          >
            <img
              src="https://res.cloudinary.com/glovoapp/w_60,h_60,c_fit,f_auto,q_auto:best/StoreCategories/o2g7fmssjbb4cvpqu1jz"
              alt=""
            />
            <p
              class="
                text-[12px]
                leading-[14px]
                font-light
                text-black text-center
              "
            >
              Bilo šta
            </p>
          </div>
          <div
            class="
              flex flex-col
              items-center
              justify-center
              gap-[5px]
              rounded-full
              h-[114px]
              w-[114px]
              bg-white
              cursor-pointer
              hover:scale-110
              transform
              transition-all
              duration-150
              ease-in-out
            "
          >
            <img
              src="https://res.cloudinary.com/glovoapp/w_60,h_60,c_fit,f_auto,q_auto:best/CategoryGroups/gtzmscjielvwxwkxnots"
              alt=""
            />
            <p
              class="
                text-[12px]
                leading-[14px]
                font-light
                text-black text-center
              "
            >
              Hrana
            </p>
          </div>
          <div
            class="
              flex flex-col
              items-center
              justify-center
              gap-[5px]
              rounded-full
              h-[114px]
              w-[114px]
              bg-white
              cursor-pointer
              hover:scale-110
              transform
              transition-all
              duration-150
              ease-in-out
            "
          >
            <img
              src="https://res.cloudinary.com/glovoapp/w_60,h_60,c_fit,f_auto,q_auto:best/StoreCategories/yn87iqomg2p9h5bwpssz"
              alt=""
            />
            <p
              class="
                text-[12px]
                leading-[14px]
                font-light
                text-black text-center
              "
            >
              Piće
            </p>
          </div>
        </div>
      </div>
    </div>
    <nearby-food :locationData="locationData" />

    <address-modal
      :showModal="showModal"
      @closeModal="showModal = false"
      @setLocationData="setLocationData"
    />
  </div>
</template>

<script>
import AddressModal from "./AddressModal.vue";
import NearbyFood from "./NearbyFood.vue";
export default {
  components: { AddressModal, NearbyFood },
  data() {
    return {
      showModal: false,
      locationData: null,
    };
  },
  methods: {
    setLocationData(value) {
      console.log("value", value);
      this.locationData = value;
      console.log(this.locationData, "this.locationData");
    },
  },
};
</script>

<style lang="scss" scoped>
</style>