<template>
  <v-toolbar
    app
    :color="primaryColor"
    dark
    dense
    clipped-left
  >
    <v-toolbar-title
      v-if="isMimicking"
      slot="extension"
    >
      You are currently mimicking {{ mimickedUser }}.
    </v-toolbar-title>

    <v-toolbar-items>
      <v-btn
        flat
        href="/"
      >
        <span class="toolbar-button-name hidden-xs-and-down">
          <img
            v-show="!isMimicking"
            id="phoenix-logo"
            src="/images/logos/298x149_white.png"
          >
          <img
            v-show="isMimicking"
            id="phoenix-logo"
            src="/images/logos/298x149_spy.png"
          >
        </span>
        <span class="toolbar-button-name hidden-md-and-down">&nbsp; HOME</span>
      </v-btn>
    </v-toolbar-items>
    <v-spacer />
    <v-toolbar-items>
      <v-btn
        flat
        :href="dashboardLink"
      >
        <v-icon>dashboard</v-icon>
        <span class="toolbar-button-name hidden-md-and-down">Dashboard</span>
      </v-btn>
      <v-btn
        flat
        href="/client_searches/more"
      >
        <v-icon>wc</v-icon>
        <span class="toolbar-button-name hidden-md-and-down">Participants</span>
      </v-btn>

      <v-menu
        offset-y
        open-on-hover
        :close-on-content-click="false"
        :nudge-width="10"
      >
        <v-btn
          slot="activator"
          flat
        >
          <v-icon>sim_card</v-icon>
          <span class="toolbar-button-name hidden-md-and-down">Providers</span>
        </v-btn>
        <v-list light>
          <v-list-tile
            v-for="(item, i) in providerMenus"
            v-show="item.hidden ? false : true"
            :key="i"
            :href="item.href"
            :color="primaryColor"
          >
            <v-list-tile-title>{{ item.title }}</v-list-tile-title>
          </v-list-tile>
        </v-list>
      </v-menu>

      <v-btn
        class="hidden-sm-and-down"
        flat
        href="/waiting_lists"
      >
        <v-icon>list</v-icon>
        <span class="toolbar-button-name hidden-md-and-down">Waiting Lists</span>
      </v-btn>

      <v-btn
        v-show="isAdmin"
        class="hidden-sm-and-down"
        flat
        href="/reports"
      >
        <v-icon>bar_chart</v-icon>
        <span class="toolbar-button-name hidden-md-and-down">Reports</span>
      </v-btn>
      <v-btn
        v-show="isAdmin"
        class="hidden-sm-and-down"
        flat
        href="/administrative_actions"
      >
        <v-icon>settings</v-icon>
        <span class="toolbar-button-name hidden-md-and-down">Admin</span>
      </v-btn>

      <v-btn
        flat
        href="/conversations"
      >
        <v-icon>inbox</v-icon>
        <span class="toolbar-button-name hidden-md-and-down">Inbox</span>
      </v-btn>

      <v-btn
        flat
        href="/issues?page=1&sort=created_at"
      >
        <v-icon>bug_report</v-icon>
        <span class="toolbar-button-name hidden-md-and-down">Issues</span>
      </v-btn>

      <v-btn
        flat
        href="/policy"
      >
        <v-icon>help</v-icon>
        <span class="toolbar-button-name hidden-md-and-down">Help</span>
      </v-btn>

      <v-menu
        bottom
        offset-y
        open-on-hover
        :close-on-content-click="false"
      >
        <v-btn
          slot="activator"
          flat
        >
          <v-avatar
            :color="secondaryColor"
            size="40"
          >
            <span class="title">{{ userInitials }}</span>
          </v-avatar>
        </v-btn>
        <v-list light>
          <v-list-tile
            v-for="(item, i) in profileMenus"
            v-show="!item.hidden"
            :key="i"
            :href="item.href"
            :color="primaryColor"
          >
            <v-list-tile-action>
              <v-icon :color="secondaryColor">
                {{ item.icon }}
              </v-icon>
            </v-list-tile-action>
            <v-list-tile-title>{{ item.title }}</v-list-tile-title>
          </v-list-tile>
        </v-list>
      </v-menu>
    </v-toolbar-items>
  </v-toolbar>
</template>

<script>
  export default {
    data: () => ({
      // User Variables
      isAdmin: false,
      isMimicking: false,
      userInitials: '',
      dashboardLink: '/',
      mimickedUser: '',

      // Styling Variables
      primaryColor: '',
      secondaryColor: '',

      settings: true,

      cards: [
        {
          title: 'Pre-fab homes',
          src: 'https://cdn.vuetifyjs.com/images/cards/house.jpg',
          flex: 12
        },
        {
          title: 'Favorite road trips',
          src: 'https://cdn.vuetifyjs.com/images/cards/road.jpg',
          flex: 6
        },
        {
          title: 'Best airlines',
          src: 'https://cdn.vuetifyjs.com/images/cards/plane.jpg',
          flex: 6
        }
      ],
      colors: [
        'red',
        'pink',
        'purple',
        'deep-purple',
        'indigo',
        'blue',
        'light-blue',
        'cyan',
        'teal',
        'green',
        'light-green',
        'lime',
        'yellow',
        'amber',
        'orange',
        'deep-orange',
        'brown',
        'blue-grey'
      ],

      profileMenus: [
        {
          hidden: false,
          title: 'My Profile',
          icon: 'account_circle',
          href: '/'
        },
        {
          hidden: false,
          title: 'Mimic User',
          icon: 'people_outline',
          href: '/mimicked_users/new'
        },
        {
          hidden: true,
          title: 'Stop Mimicking',
          icon: 'people_outline',
          href: '/mimicked_users/destroy'
        },
        {
          hidden: false,
          title: 'Sign Out',
          icon: 'logout',
          href: '/logout'
        }
      ],

      providerMenus: [
        {
          hidden: false,
          title: 'Browse Providers',
          href: '/provider_searches'
        },
        {
          hidden: false,
          title: 'Search for Providers',
          href: '/provider_searches/new'
        },
        {
          hidden: false,
          title: 'Browse Local Providers',
          href: '/local_provider_searches'
        },
        {
          hidden: false,
          title: 'Search for Local Providers',
          href: '/local_provider_searches/new'
        },
        {
          hidden: true,
          title: 'Provider Compliance',
          href: '/provider_compliance_provider_searches/new'
        },
        {
          hidden: false,
          title: 'Browse Provider Maps',
          href: '/provider_maps'
        },
        {
          hidden: false,
          title: 'Browse Provider Groups',
          href: '/provider_groups'
        }
      ]
    }),

    watch: {
      settings (newSettings) {
        console.log(newSettings)
      }
    }
  }
</script>
