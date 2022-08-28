<template>
  <Transition name="modal">
    <div v-if="showModal" class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div
            :class="
              activeStep == 2 ? 'justify-between ml-[30px]' : 'justify-end'
            "
            class="modal-header mr-[30px] pt-[30px] flex items-center"
          >
            <img
              @click="activeStep = 1"
              v-show="activeStep == 2"
              :src="require('../assets/images/arrow-left.png')"
              class="cursor-pointer"
            />
            <img
              class="cursor-pointer"
              data-v-7fa6eaf0=""
              @click="$emit('closeModal')"
              src="https://res.cloudinary.com/glovoapp/image/fetch//q_auto/https://glovoapp.com/images/close-icon.svg"
            />
          </div>

          <div class="modal-body pb-[72px] px-[48px] pt-[20px]">
            <h1
              class="
                text-[28px]
                leading-[32px]
                text-black text-center
                font-bold
                mb-[40px]
              "
            >
              {{
                activeStep == 1
                  ? "Unesi adresu za dostavu"
                  : "Postavi čiodu na svoju adresu na mapi"
              }}
            </h1>
            <div v-if="activeStep == 1" class="flex flex-row gap-[25px]">
              <div
                class="flex-[50%] justify-between items-stretch flex flex-col"
              >
                <div>
                  <div class="flex flex-row items-center gap-[10px] relative">
                    <img
                      data-v-653ea48f=""
                      src="https://res.cloudinary.com/glovoapp/image/fetch///https://glovoapp.com/images/glyphs/flag_dark-grey.svg"
                      class="text-field__icon"
                    />

                    <input
                      type="text"
                      v-model="streetName"
                      @focus="inputFocused = true"
                      @blur="inputFocused = false"
                      class="
                        bg-transparent
                        z-50
                        w-[75%]
                        caret-[#00A082FF]
                        border-b-[1px]
                        focus:outline-none
                        focus:border-b-[2px]
                        focus:border-b-[#00A082FF]
                        border-[#C1C1C1FF] border-t-0 border-l-0 border-r-0
                        pb-[10px]
                        pr-[24px]
                        text-[18px]
                        font-light
                        leading-[24px]
                        text-[#6e6e6e]
                      "
                    />
                    <span
                      :class="inputFocused || (streetName && streetName.length) ? 'translate-x-[-36px] translate-y-[-28px] text-[14px]' : ''"
                      class="
                        absolute
                        transition-all
                        left-[40px] text-[18px]
                        duration-200
                        ease-in-out
                        font-light
                        leading-[24px]
                        text-[#6e6e6e]
                        mb-[10px]
                        z-10
                      "
                    >
                      Pretraži ulicu, gradove, kvartov..
                    </span>
                  </div>
                  <div
                    @click="getUserLocation"
                    class="
                      cursor-pointer
                      flex flex-row
                      gap-[8px]
                      items-center
                      justify-center
                      w-full
                      pt-[30px]
                    "
                  >
                    <img
                      data-v-1c78e0bb=""
                      loading="lazy"
                      src="https://res.cloudinary.com/glovoapp/image/fetch//q_auto/https://glovoapp.com/images/svg/location.svg"
                    />
                    <p
                      class="text-[#00a082] text-[14px] leading-[22px] mr-[15%]"
                    >
                      Koristi trenutnu lokaciju
                    </p>
                  </div>
                  <p
                    class="text-red-400 text-[14px] leading-[22px] pt-[20px]"
                    v-if="errorMsg"
                  >
                    {{ errorMsg }}
                  </p>
                </div>
                <div>
                  <p
                    @click="activeStep = 2"
                    class="
                      cursor-pointer
                      text-[#00a082] text-[14px]
                      leading-[22px]
                      ml-[40px]
                    "
                  >
                    Ili postavi svoju lokaciju na mapi
                  </p>
                </div>
              </div>
              <div class="flex-[50%]">
                <img
                  data-v-1cf16b17=""
                  data-test-id="location-map-placeholder"
                  src="https://res.cloudinary.com/glovoapp/image/fetch//f_auto,q_auto/https://glovoapp.com/images/map-placeholder.jpg"
                  class="location-map fluid"
                />
              </div>
            </div>
            <div
              v-if="activeStep == 2"
              class="flex flex-col items-center justify-center"
            >
              <div class="relative">
                <vl-map
                  :load-tiles-while-animating="true"
                  :load-tiles-while-interacting="true"
                  data-projection="EPSG:4326"
                  style="height: 308px; width: 560px; cursor: grab"
                >
                  <vl-view
                    :zoom.sync="zoom"
                    :center.sync="center"
                    :rotation.sync="rotation"
                  ></vl-view>

                  <!-- <vl-geoloc @update:position="geolocPosition = $event">
                  <template #default="geoloc">
                    <vl-feature v-if="geoloc.position" id="position-feature">
                      <vl-geom-point
                        :coordinates="center"
                      ></vl-geom-point>
                      <vl-style-box>
                        <vl-style-icon
                          :src="require('../assets/images/marker.svg')"
                        ></vl-style-icon>
                      </vl-style-box>
                    </vl-feature>
                  </template>
                </vl-geoloc> -->

                  <vl-layer-tile id="osm">
                    <vl-source-osm></vl-source-osm>
                  </vl-layer-tile>
                </vl-map>
                <img
                  v-if="isLocationDataEmpty != 'Van naše oblasti dostave'"
                  :src="require('../assets/images/marker.svg')"
                  class="
                    absolute
                    top-[50%]
                    left-[50%]
                    translate-x-[-50%] translate-y-[-50%]
                  "
                />
                <img
                  v-else
                  :src="require('../assets/images/marker-disabled.svg')"
                  class="
                    absolute
                    top-[50%]
                    left-[50%]
                    translate-x-[-50%] translate-y-[-50%]
                  "
                />
              </div>

              <div class="flex items-center gap-[10px] pt-[32px] w-[560px]">
                <div>
                  <img
                    v-if="isLocationDataEmpty != 'Van naše oblasti dostave'"
                    data-v-225d1950=""
                    data-test-id="address-details-card-img"
                    src="https://res.cloudinary.com/glovoapp/image/fetch//q_auto/https://glovoapp.com/images/svg/marker--small.svg"
                    class="media__img"
                  />
                  <img
                    v-else
                    data-v-225d1950=""
                    data-test-id="address-details-card-img"
                    src="https://res.cloudinary.com/glovoapp/image/fetch//q_auto/https://glovoapp.com/images/svg/marker--small--disabled.svg"
                    class="media__img"
                  />
                </div>
                <div
                  class="flex flex-col items-stretch justify-between gap-[6px]"
                >
                  <p class="text-[16px] font-light leading-[20px] text-black">
                    {{ isLocationDataEmpty }}
                  </p>
                  <span
                    v-if="
                      isLocationDataEmpty !=
                      'Postavi čiodu na tačnu lokaciju da pomogneš dostavljaču da pronađe adresu'
                    "
                    class="text-[14px] leading-[18px] text-[#6e6e6e] font-light"
                  >
                    {{ locationJson.city ? locationJson.city + ", " : "" }}
                    {{ locationJson.country }}

                    {{ geolocPosition }}
                  </span>
                </div>
              </div>
              <div class="flex items-center justify-center pt-[40px]">
                <button
                  @click="saveAddress()"
                  :disabled="
                    isLocationDataEmpty == 'Van naše oblasti dostave' ||
                    isLocationDataEmpty ==
                      'Postavi čiodu na tačnu lokaciju da pomogneš dostavljaču da pronađe adresu'
                  "
                  :class="
                    isLocationDataEmpty == 'Van naše oblasti dostave' ||
                    isLocationDataEmpty ==
                      'Postavi čiodu na tačnu lokaciju da pomogneš dostavljaču da pronađe adresu'
                      ? 'bg-[#C1C1C1] cursor-not-allowed'
                      : 'bg-[#00A082] hover:bg-[#00846BFF]'
                  "
                  class="
                    text-white
                    font-bold
                    text-[18px]
                    leading-[21px]
                    rounded-[48px]
                    text-center
                    w-[340px]
                    h-[48px]
                    shadow-xl
                  "
                >
                  Potvrdi adresu
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script>
export default {
  props: {
    showModal: Boolean,
  },
  data() {
    return {
      streetName: null,
      inputFocused: false,
      activeStep: 1,
      zoom: 2,
      center: [0, 0],
      rotation: 0,
      errorMsg: '',
      geolocPosition: undefined,
      locationJson: {
        city: null,
        country: null,
        street: null,
        houseNumber: null,
        postalCode: null,
      },
      savedAddress: {},
    };
  },
  computed: {
    isLocationDataEmpty() {
      console.log(this.locationJson, "we dont talk about it");

      var isEmpty = Object.values(this.locationJson).every(
        (x) => x === null || x === ""
      );
      if (isEmpty) {
        return "Postavi čiodu na tačnu lokaciju da pomogneš dostavljaču da pronađe adresu";
      } else if (
        (!isEmpty && !this.locationJson.city) ||
        !this.locationJson.street
      ) {
        return "Van naše oblasti dostave";
      } else {
        return this.createStreetName();
      }
    },
  },
  methods: {
    focusInput(event) {
      console.log(event, 'event')
    },
    async getUserLocation() {
      await navigator.geolocation.getCurrentPosition(
        (position) => {
          var pos = [position.coords.latitude, position.coords.longitude];
          this.getLocation(pos);
          if (!this.locationJson.city || !this.locationJson.street) {
            this.errorMsg =
              "Dostava nedostupna za traženu lokaciju. Pokušajte da obarete svoju lokaciju na mapi.";
          } else {
            this.saveAddress();
          }
        },
        (error) => {
          console.log(error.message);
        }
      );
    },

    saveAddress() {
      console.log("zasto se desava saveAddress na promjeni");
      Object.assign(this.savedAddress, this.locationJson);
      this.$emit("closeModal");
      this.$emit("setLocationData", this.locationJson);
    },

    createStreetName() {
      var name = "";
      if (this.locationJson.street && this.locationJson.houseNumber) {
        name +=
          this.locationJson.street + " " + this.locationJson.houseNumber + ", ";
      } else if (this.locationJson.street) {
        name += this.locationJson.street + ", ";
      } else if (this.locationJson.houseNumber) {
        name += this.locationJson.houseNumber + ", ";
      }
      if (this.locationJson.city && this.locationJson.postalCode) {
        name +=
          this.locationJson.city + " " + this.locationJson.postalCode + ", ";
      } else if (this.locationJson.city) {
        name += this.locationJson.city + ", ";
      } else if (this.locationJson.postalCode) {
        name += this.locationJson.postalCode + ", ";
      }
      name += this.locationJson.country;
      return name;
    },
    async getLocation(coordinates) {
      await fetch(
        "http://nominatim.openstreetmap.org/reverse?format=json&lon=" +
          coordinates[0] +
          "&lat=" +
          coordinates[1]
      )
        .then((response) => response.json())
        .then((location) => {
          this.locationJson.city = location.address.city
            ? location.address.city
            : null;
          this.locationJson.country = location.address.country
            ? location.address.country
            : null;
          this.locationJson.street = location.address.road
            ? location.address.road
            : null;
          this.locationJson.houseNumber = location.address.house_number
            ? location.address.house_number
            : null;
          this.locationJson.postalCode = location.address.postcode
            ? location.address.postcode
            : null;
          this.locationJson.lat = coordinates[0];
          this.locationJson.long = coordinates[1];
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  watch: {
    center(newVal) {
      this.getLocation(newVal);
    },
  },
};
</script>


<style>
.location-map {
  width: 100%;
  position: relative;
  height: 308px;
  width: 308px;
  -o-object-fit: cover;
  object-fit: cover;
  overflow: hidden;
  border-radius: 4px;
}

.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 800px;
  margin: 0px auto;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-default-button {
  float: right;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>