<template>
  <q-layout view="lHh Lpr lFf">
    <q-header>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />
      </q-toolbar>
      <div class="q-px-lg q-pt-xl q-mb-md">
        <div class="text-h3">Todo</div>
        <!-- <div class="text-subtitle1">{{ todaysDate }}</div> -->
        <div class="text-subtitle1">{{ new Date().toLocaleString() }}</div>
      </div>
      <q-img src="/mountains.jpg" class="header-image absolute-top" />
    </q-header>

    <q-drawer v-model="drawer" show-if-above :width="250" :breakpoint="600">
      <q-scroll-area
        style="
          height: calc(100% - 192px);
          margin-top: 192px;
          border-right: 1px solid #ddd;
        "
      >
        <q-list padding>
          <q-item clickable v-ripple to="/" exact>
            <q-item-section avatar>
              <q-icon name="list" />
            </q-item-section>

            <q-item-section> Todo </q-item-section>
          </q-item>

          <q-item clickable v-ripple to="/help" exact>
            <q-item-section avatar>
              <q-icon name="help" />
            </q-item-section>

            <q-item-section> Help </q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>

      <q-img class="absolute-top" src="/mountains.jpg" style="height: 192px">
        <div class="absolute-bottom bg-transparent">
          <q-avatar size="56px" class="q-mb-sm">
            <img src="/ninja.png" />
          </q-avatar>
          <div class="text-weight-bold">Stephen Lai</div>
          <div>@stephen__lai</div>
        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { date } from "quasar";

import { defineComponent, ref, computed } from "vue";

export default defineComponent({
  setup() {
    const drawer = ref(false);

    const toggleLeftDrawer = () => {
      drawer.value = !drawer.value;
    };

    const todaysDate = computed(() => {
      let timestamp = Date.now();
      return date.formatDate(timestamp, "dddd D MMMM");
    });

    return {
      drawer,
      toggleLeftDrawer,
      todaysDate,
    };
  },
});
</script>

<style lang="scss" scoped>
.header-image {
  height: 100%;
  z-index: -1;
  opacity: 0.2;
  filter: grayscale(100%);
}
</style>
