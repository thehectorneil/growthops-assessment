<template>
  <q-layout view="hHh lpR fFf">
    <q-header flat>
      <q-toolbar class="bg-brand">
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
          v-show="$q.screen.lt.md"
        />

        <q-avatar>
          <img src="../assets/logo.png" />
        </q-avatar>

        <q-space></q-space>

        <q-tabs v-model="tab" v-show="!$q.screen.lt.md">
          <q-tab class="tab" name="tab1" label="Our Products" />
          <q-tab class="tab" name="tab2" label="About Us" />
          <q-tab class="tab" name="tab3" label="Live Better" />
          <q-tab class="tab" name="tab3" label="Claims & Support" />
          <q-tab class="tab" name="tab3" label="My Account" />
        </q-tabs>
        <q-space></q-space>
        <q-space></q-space>
        <q-space></q-space>
        <q-space></q-space>
        <q-space></q-space>
        <q-space></q-space>
        <q-space></q-space>
        <q-space></q-space>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" bordered v-show="$q.screen.lt.md">
      <q-list>
        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";
import EssentialLink from "components/EssentialLink.vue";

const linksList = [
  {
    title: "Our Products",
    caption: "quasar.dev",
    icon: "school",
    link: "https://quasar.dev",
  },
  {
    title: "About Us",
    caption: "github.com/quasarframework",
    icon: "code",
    link: "https://github.com/quasarframework",
  },
  {
    title: "Live Better",
    caption: "chat.quasar.dev",
    icon: "chat",
    link: "https://chat.quasar.dev",
  },
  {
    title: "Claims & Support",
    caption: "forum.quasar.dev",
    icon: "record_voice_over",
    link: "https://forum.quasar.dev",
  },
  {
    title: "My Account",
    caption: "@quasarframework",
    icon: "rss_feed",
    link: "https://twitter.quasar.dev",
  },
];

export default defineComponent({
  name: "MainLayout",

  components: {
    EssentialLink,
  },

  setup() {
    const leftDrawerOpen = ref(false);

    return {
      tab: ref(""),
      essentialLinks: linksList,
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
    };
  },
});
</script>

<style scoped>
.bg-brand {
  background: #d41444 !important;
  height: 75px !important;
}
</style>
