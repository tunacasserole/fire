<template>
  <v-navigation-drawer
    app
    permanent
    clipped
    width="260"
    :style="isMimicking ? 'margin-top: 82px; max-height: calc(100% - 82px) !important;' : 'margin-top: 48px; max-height: calc(100% - 48px) !important;'"
    :mini-variant="mini"
    mini-variant-width="50"
  >
    <v-list v-if="!mini">
      <v-list-group
        v-for="menu in menus"
        v-show="!menu.hidden"
        :key="menu.title"
        v-model="menu.active"
        :prepend-icon="menu.icon"
        no-action
      >
        <v-list-tile slot="activator">
          <v-list-tile-content>
            <v-list-tile-title>{{ menu.title }}</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>

        <v-list-tile
          v-for="subMenu in menu.menus"
          v-show="subMenu.hidden ? false : true"
          :key="subMenu.title"
          :href="subMenu.href"
        >
          <v-list-tile-content>
            <v-list-tile-title>{{ subMenu.title }}</v-list-tile-title>
          </v-list-tile-content>

          <v-list-tile-action>
            <v-icon>{{ subMenu.icon }}</v-icon>
          </v-list-tile-action>
        </v-list-tile>
      </v-list-group>

      <v-list-tile
        style="position:absolute;bottom:0;width:-webkit-fill-available"
        @click.stop="mini = !mini"
      >
        <v-list-tile-action>
          <v-btn icon>
            <v-icon>arrow_back</v-icon>
          </v-btn>
        </v-list-tile-action>

        <v-list-tile-content>
          <v-list-tile-title>Collapse sidebar</v-list-tile-title>
        </v-list-tile-content>

        <v-list-tile-action>
          <v-btn
            icon
            @click.stop="togglePalette"
          >
            <v-icon>arrow_back</v-icon>
          </v-btn>
        </v-list-tile-action>
      </v-list-tile>
    </v-list>

    <v-list v-if="mini">
      <v-menu
        v-for="menu in menus"
        v-show="!menu.hidden"
        :key="menu.title"
        v-model="menu.active"
        right
        :nudge-right="42"
        z-index="1"
      >
        <v-btn
          slot="activator"
          icon
          flat
        >
          <v-icon>{{ menu.icon }}</v-icon>
        </v-btn>
        <v-list light>
          <v-list-tile
            v-for="(item, i) in menu.menus"
            v-show="item.hidden ? false : true"
            :key="i"
            :href="item.href"
            :color="primaryColor"
          >
            <v-list-tile-title>{{ item.title }}</v-list-tile-title>
          </v-list-tile>
        </v-list>
      </v-menu>
      <span
        style="position:absolute;bottom:10px"
        @click.stop="mini = !mini"
      >
        <v-list-tile-action>
          <v-btn icon>
            <v-icon>arrow_forward</v-icon>
          </v-btn>
        </v-list-tile-action>
      </span>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
// Utilities
  import { mapMutations } from 'vuex'

  export default {
    data: () => ({
      isMimicking: false,
      mini: false,
      // Menus and sub menus
      menus: [
        {
          hidden: false,
          icon: 'group',
          title: 'Participants',
          menus: [
            {
              title: 'Search for Participants',
              href: '/client_searches/new'
            },
            {
              title: 'New PCSC Brief Screener',
              href: '/pcsc_screens/new'
            }
          ]
        },
        {
          hidden: false,
          icon: 'sim_card',
          title: 'Providers',
          menus: [
            {
              title: 'Browse Providers',
              href: '/provider_searches'
            },
            {
              title: 'Search for Providers',
              href: '/provider_searches/new'
            },
            {
              title: 'Browse Local Providers',
              href: '/local_provider_searches'
            },
            {
              title: 'Search for Local Providers',
              href: '/local_provider_searches/new'
            },
            {
              hidden: true,
              title: 'Provider Compliance',
              href: '/provider_compliance_provider_searches/new'
            },
            {
              title: 'Browse Provider Maps',
              href: '/provider_maps'
            },
            {
              title: 'Browse Provider Groups',
              href: '/provider_groups'
            }
          ]
        },
        {
          hidden: false,
          icon: 'account_circle',
          title: 'Users',
          menus: [
            {
              title: 'Search for Users',
              href: '/user_searches/new'
            },
            {
              hidden: true,
              title: 'Work with User Requests',
              href: '/user_requests'
            },
            {
              hidden: true,
              title: 'User Rosters',
              href: '/user_rosters'
            }
          ]
        },
        {
          hidden: true,
          icon: 'directions_walk',
          title: 'Workers',
          menus: [
            {
              title: 'Search Workers',
              href: '/staff'
            },
            {
              title: 'Missed Visits',
              href: '/missed_visits'
            }
          ]
        },
        {
          hidden: true,
          icon: 'ballot',
          title: 'Claims',
          menus: [
            {
              title: 'Search for Claims',
              href: '/claims/search'
            },
            {
              title: 'Outstanding Resolutions',
              href: '/claim_resolutions'
            }
          ]
        },
        {
          hidden: true,
          icon: 'comment',
          title: 'Feedback',
          menus: [
            {
              title: 'Issues Tracker',
              href: '/issues'
            },
            {
              title: 'Complaint Log',
              href: '/complaints'
            }
          ]
        }
      ],
      primaryColor: 'blue'
    }),

    watch: {
      mini (newMini) {
        localStorage.mini = newMini
      }
    },

    mounted () {
      if (localStorage.mini) {
        this.mini = localStorage.mini === 'true'
      }

      this.$axios.get('/navigation/show').then(response => {
        // User Data
        this.profileMenus[0].href =
          '/users/' + response.data.current_user_id + '/edit'
        this.isMimicking = response.data.is_mimicking
        this.userInitials = response.data.current_user_initials
        this.dashboardLink = response.data.mimicked_user_dashboard_url
        this.isAdmin = response.data.is_admin
        this.mimickedUser = response.data.mimicked_user

        // Menu Security
        // Mimicking Menus
        this.profileMenus[1].hidden = response.data.is_mimicking // Mimick User
        this.profileMenus[2].hidden = !response.data.is_mimicking // Stop Mimicking

        // Provider Compliance Menus
        this.providerMenus[4].hidden = !response.data.is_claims
        this.menus[1].menus[4].hidden = !response.data.is_claims

        // User menus
        this.menus[2].menus[1].hidden = !response.data.is_user_admin
        this.menus[2].menus[2].hidden = !response.data.is_user_admin

        this.menus[3].hidden = !response.data.is_claims // Worker menus
        this.menus[4].hidden = !response.data.is_claims // Claims menus
      })
    },

    methods: {
      ...mapMutations(['togglePalette'])
    }
  }
</script>
