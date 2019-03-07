<template>
  <v-app>
    <core-drawer />

    <core-toolbar />
  </v-app>
</template>

<script>
  export default {
    components: {
      CoreDrawer: () => import('@/components/core/Drawer'),
      CoreToolbar: () => import('@/components/core/Toolbar')
    },

    computed: {
      currentId () {
        return window.location.pathname.split('/')[2]
      },
      clientVisible () {
        return window.location.pathname.split('/')[1] === 'clients'
      },
      providerVisible () {
        return window.location.pathname.split('/')[1] === 'providers'
      }
    }
  }
</script>
