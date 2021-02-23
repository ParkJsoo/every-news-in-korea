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
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          모두의 뉴스
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="200"
      :breakpoint="400"
    >
      <q-scroll-area
        style="height: calc(100% - 150px); margin-top: 150px; border-right: 1px solid #ddd"
      >
        <q-list padding>
          <q-item clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="fas fa-home" color="primary" />
            </q-item-section>

            <q-item-section>
              홈
            </q-item-section>
          </q-item>

          <router-link to="/about">
            <q-item clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="fas fa-user" color="secondary" />
              </q-item-section>

              <q-item-section>
                프로필
              </q-item-section>
            </q-item>
          </router-link>
        </q-list>
      </q-scroll-area>

      <q-img
        class="absolute-top"
        src="https://picsum.photos/400"
        style="height: 150px"
      >
        <div class="absolute-bottom bg-transparent">
          <q-avatar size="56px" class="q-mb-sm">
            <img src="https://picsum.photos/100" />
          </q-avatar>
          <div class="text-weight-bold">Jeongsoo Park</div>
          <div>@ParkJsoo</div>
        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: "MainLayout",
  components: {},
  data() {
    return {
      leftDrawerOpen: false
    };
  }
};
</script>

<style lang="scss">
a:link,
a:hover,
a:active,
a:visited {
  text-decoration: none;
  color: black;
}
</style>
