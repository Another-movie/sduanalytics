<template>
  <section>
    <b-sidebar
      :fullheight="fullheight"
      :fullwidth="fullwidth"
      :overlay="overlay"
      :right="right"
      :open.sync="open"
    >
      <div class="p-1">
        <b-menu>
          <b-menu-list label="Menu">
            <b-menu-item icon="information-outline" label="Info"></b-menu-item>
            <b-menu-item icon="settings">
              <template slot="label" slot-scope="props">
                Administrator
                <b-icon
                  class="is-pulled-right"
                  :icon="props.expanded ? 'menu-down' : 'menu-up'"
                ></b-icon>
              </template>
              <b-menu-item icon="account" label="Users"></b-menu-item>
              <b-menu-item icon="cellphone-link">
                <template slot="label">
                  Devices
                  <b-dropdown
                    aria-role="list"
                    class="is-pulled-right"
                    position="is-bottom-left"
                  >
                    <b-icon icon="dots-vertical" slot="trigger"></b-icon>
                    <b-dropdown-item aria-role="listitem"
                      >Action</b-dropdown-item
                    >
                    <b-dropdown-item aria-role="listitem"
                      >Another action</b-dropdown-item
                    >
                    <b-dropdown-item aria-role="listitem"
                      >Something else</b-dropdown-item
                    >
                  </b-dropdown>
                </template>
              </b-menu-item>
              <b-menu-item
                icon="cash-multiple"
                label="Payments"
                disabled
              ></b-menu-item>
            </b-menu-item>
            <router-link to="/services" tag='div' replace>
              <b-menu-item icon="developer-board" label="Services">
              </b-menu-item>
            </router-link>
          </b-menu-list>
          <b-menu-list>
            <b-menu-item label="Expo" icon="link"></b-menu-item>
          </b-menu-list>
          <b-menu-list label="Actions">
            <b-menu-item label="Logout"></b-menu-item>
          </b-menu-list>
        </b-menu>
      </div>
    </b-sidebar>
    <b-button
      @click="open = true"
      icon-left="menu"
      id="burger-button"
    ></b-button>
  </section>
</template>

<script>
export default {
  data() {
    return {
      open: false,
      overlay: false,
      fullheight: true,
      fullwidth: false,
      right: true,
      icon: "menu",
    };
  },
};
</script>

<style>
#burger-button {
  border: none;
  outline: none;
  padding: 0;
  font-size: 1.5rem;
  margin: 0 20px 0 20px;
}

.b-sidebar .sidebar-content {
  background-color: #232931 !important;
}
.p-1 {
  padding: 1em;
}
</style>
