<template>
  <div class="palette">
    <h1>This palette allows you to design your own color scheme.</h1>
    <v-container
      grid-list-md
      text-xs-center
      fluid
    >
      <v-layout
        row
        wrap
      >
        <v-flex
          xs7
          mx-5
        >
          <v-layout
            row
            wrap
          >
            <v-flex
              v-for="c in colors"
              :key="c"
              xs2
            >
              <v-card
                ripple
                dark
                :color="c"
                height="300px"
                @click="primaryColor == '' ? primaryColor = c : secondaryColor = c"
              >
                <v-card-text>{{ c }}</v-card-text>
              </v-card>
            </v-flex>
          </v-layout>
        </v-flex>
        <v-flex xs4>
          <v-toolbar
            :color="primaryColor"
            dark
          >
            <v-toolbar-side-icon />
            <v-toolbar-title>Theme Preview</v-toolbar-title>
            <v-spacer />
            <v-avatar
              :color="secondaryColor"
              size="40"
            >
              <span class="title">{{ userInitials }}</span>
            </v-avatar>
          </v-toolbar>

          <v-card>
            <v-container
              fluid
              grid-list-md
            >
              <v-layout
                row
                wrap
              >
                <v-flex
                  v-for="card in cards"
                  :key="card.title"
                  v-bind="{ [`xs${card.flex}`]: true }"
                >
                  <v-card light>
                    <v-img
                      light
                      :src="card.src"
                      height="200px"
                    >
                      <v-container
                        fluid
                        pa-2
                      >
                        <v-layout>
                          <v-flex
                            xs12
                            align-end
                            flexbox
                          >
                            <span
                              class="headline white--text"
                              v-text="card.title"
                            />
                          </v-flex>
                        </v-layout>
                      </v-container>
                    </v-img>

                    <v-card-actions>
                      <v-spacer />
                      <v-btn icon>
                        <v-icon
                          large
                          :color="secondaryColor"
                        >
                          favorite_border
                        </v-icon>
                      </v-btn>
                      <v-btn icon>
                        <v-icon
                          large
                          :color="secondaryColor"
                        >
                          bookmark_border
                        </v-icon>
                      </v-btn>
                      <v-btn icon>
                        <v-icon
                          large
                          :color="secondaryColor"
                        >
                          share
                        </v-icon>
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-flex>
              </v-layout>
            </v-container>
          </v-card>

          <v-layout
            row
            wrap
            ma-5
          >
            <v-flex
              xs4
              mr-4
              ml-5
            >
              <v-card
                dark
                :color="primaryColor"
                height="180px"
                @click="primaryColor = ''"
              >
                <v-divider light />
                <v-card-actions class="pa-3">
                  Reset the Primary Color
                  <v-spacer />
                </v-card-actions>
              </v-card>
            </v-flex>
            <v-flex xs4>
              <v-card
                dark
                :color="secondaryColor"
                height="180px"
                @click="secondaryColor = ''"
              >
                <v-divider light />
                <v-card-actions class="pa-3">
                  Reset the Secondary Color
                  <v-spacer />
                </v-card-actions>
              </v-card>
            </v-flex>
          </v-layout>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        primaryColor: '',
        secondaryColor: '',
        userInitials: 'XX',
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
        ]
      }
    }
  }
</script>
