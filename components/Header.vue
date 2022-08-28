<template>
  <div class="w-full bg-[#ffc244] bg-generator">
    <div
      class="flex justify-between gap-4 py-[30px] px-[7.5%]"
    >
      <img
        src="https://res.cloudinary.com/glovoapp/image/fetch//q_auto/https://glovoapp.com/images/logo_green.svg"
        data-test-id="header-logo-image"
        alt=""
        style="width: 120px; height: 38px"
        class="cursor-pointer"
      />
      <button
        class="
          bg-[#00A082]
          text-white
          font-bold
          text-[18px]
          leading-[21px]
          rounded-[48px]
          text-center
          min-w-[138px]
          h-[48px]
          hover:bg-[#00846BFF]
          shadow-xl
        "
      >
        Započni
      </button>
    </div>
    <address-picker />
  </div>
</template>

<script>
import AddressPicker from "./AddressPicker.vue";
export default {
  components: { AddressPicker },
  name: "Header",
};
</script>
<style scoped>
</style>
