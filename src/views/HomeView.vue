<script setup lang="ts">
import { RouterLink, RouterView } from "vue-router";
import axios from "axios";
import { onUnmounted, ref } from "vue";
import { onMounted } from "vue";

const glasses = ref({
  total: 0,
  limit: 12,
  page: 1,
  selected: 0,
  collections: [],
  items: [],
});

const scrollComponent = ref(null);
const isLoading = ref(true);

const filters = ref({
  sorts: [],
  pages: [],
  lens_variant_prescriptions: [],
  lens_variant_types: [],
  glass_variant_frame_variant_colour_tag_configuration_names: [],
  frame_variant_home_trial_available: null,
});
const base_url =
  "https://staging-api.bloobloom.com/user/v1/sales_channels/website/collections/spectacles-men/glasses";

onMounted(() => {
  loadGlasses();

  window.onscroll = () => {

    const {
      scrollTop,
      scrollHeight,
      clientHeight
    } = document.documentElement;

    if (scrollTop + clientHeight >= scrollHeight - 5 &&
      !isLoading.value &&
      glasses.value.selected < glasses.value.total)
      loadMore();
  }
});

onUnmounted(() => {
  //window.removeEventListener("scroll", handleScroll)
});

const loadGlasses = () => {
  axios
    .get(base_url + "?page[limit]=" + glasses.value.limit)
    .then((r) => {
      isLoading.value = false;
      glasses.value.selected = r.data.glasses.length;
      glasses.value.items = r.data.glasses;
      glasses.value.total = r.data.meta.total_count;
    })
    .catch((e) => {
      isLoading.value = false;

    });
};

const loadMore = () => {
  isLoading.value = true;
  const page_ = glasses.value.page + 1;
  axios
    .get(
      base_url +
      "?page[limit]=" +
      glasses.value.limit +
      "&page[number]=" +
      page_
    )
    .then((r) => {
      glasses.value.selected += r.data.glasses.length;

      for (var i = 0; i < r.data.glasses.length; i++) {
        glasses.value.items.push(r.data.glasses[i]);
      }

      glasses.value.total = r.data.meta.total_count;
      glasses.value.page = page_;
      isLoading.value = false;
    })
    .catch((e) => {
      isLoading.value = false;
    });
};
</script>

<template>
  <main>
    <section>
      <nav>
        <div class="grid-container">
          <div class="grid-item grid-item-center">
            TRIAL AVAILABLE <input type="checkbox" />
          </div>
          <div class="grid-item grid-item-center">SPECTACLES WOMEN</div>
          <div class="grid-item end-items">
            <div class="grid-item end-item">Filter</div>
            <div class="grid-item end-item">Search</div>
          </div>
        </div>

        <span class="grid-main-container">
          <div class="grid-item">COLOUR</div>
          <div class="grid-item">SHAPE</div>
        </span>

        <span class="grid-main-container">
          <div class="grid-item">
            <div class="colour">
              <input type="checkbox" id="green" />
              <label for="green">Green</label>
              <input type="checkbox" id="yellow" />
              <label for="yellow">Yellow</label>
              <input type="checkbox" id="dark" />
              <label for="dark">Dark</label>
            </div>
            <div class="colour">
              <input type="checkbox" id="green" />
              <label for="green">Green</label>
              <input type="checkbox" id="yellow" />
              <label for="yellow">Yellow</label>
              <input type="checkbox" id="dark" />
              <label for="dark">Dark</label>
            </div>
          </div>
          <div class="grid-item">
            <div class="colour">
              <a href="">test1 </a>
              <a href="">test1 </a>
              <a href="">test1 </a>
              <a href="">test1 </a>
            </div>
          </div>
        </span>
        <span class="grid-container">
          <div v-for="(glasse, index) in glasses.items" :key="index" class="grid-item">
            <span class="grid-item-label">{{ glasse.name }}</span>
            <img :src="glasse.glass_variants[0].media[0].url" class="glass-full" alt="glace" />
          </div>
        </span>
        <div class="items-loader" v-if="isLoading">Loading ...</div>
      </nav>
    </section>
  </main>
</template>
<style>
main {
  padding-top: 80px;
}

.items-loader {
  font-size: 2.5rem;
  text-align: center;
  font-weight: bolder;
  margin: 10px 0;
}

.glass-full {
  width: 100%;
  object-fit: cover;
}

.grid-item-label {
  position: absolute;
  width: 100%;
  left: 0;
  right: 0;
  top: 16px;
  font-weight: bold;
  text-transform: uppercase;
}
</style>
