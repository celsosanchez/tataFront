<template>
  <v-app id="sandbox">
    <v-navigation-drawer
      v-model="primaryDrawer.model"
      :clipped="primaryDrawer.clipped"
      :floating="primaryDrawer.floating"
      :temporary="true"
      app
      overflow
    >
      <v-list nav dense>
        <v-list-item-group  active-class=" text--accent-4">
          <v-list-item v-for="item in tales" :key="item._id" @click="set(item)">
            <v-list-item-icon>
              <v-icon>mdi-script-text</v-icon>
            </v-list-item-icon>
            <v-list-item-title >{{item.Title}}</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list>
      
      
    </v-navigation-drawer>

    <v-app-bar :clipped-left="primaryDrawer.clipped" app>
      <v-app-bar-nav-icon
        v-if="primaryDrawer.type !== 'permanent'"
        @click.stop="primaryDrawer.model = !primaryDrawer.model"
      ></v-app-bar-nav-icon>
      <v-toolbar-title>Para no dejar de ser niños</v-toolbar-title>
      
    </v-app-bar>

    <v-content>
      <v-container fluid>
        <v-row align="center" justify="center">
          <v-col cols="12">
            <v-card>
              <v-card-text>
                <v-row align="center" justify="center" style="margin: 3vw">
                  <h3 style="margin-bottom: 1vw">{{activestory.Title}}</h3>
                    {{activestory.Body}}
                    <!-- <span>Scheme</span> -->
                  <!-- </v-col> -->
                  <!-- <v-col
                    cols="12"
                    md="6"
                  >
                    <span>Drawer</span>
                    <v-radio-group
                      v-model="primaryDrawer.type"
                      column
                    >
                      <v-radio
                        v-for="drawer in drawers"
                        :key="drawer"
                        :label="drawer"
                        :value="drawer.toLowerCase()"
                        primary
                      ></v-radio>
                    </v-radio-group>
                    <v-switch
                      v-model="primaryDrawer.clipped"
                      label="Clipped"
                      primary
                    ></v-switch>
                    <v-switch
                      v-model="primaryDrawer.floating"
                      label="Floating"
                      primary
                    ></v-switch>
                    <v-switch
                      v-model="primaryDrawer.mini"
                      label="Mini"
                      primary
                    ></v-switch>
                  </v-col>-->
                  <!-- <v-col
                    cols="12"
                    md="6"
                  >
                    <span>Footer</span>
                    <v-switch
                      v-model="footer.inset"
                      label="Inset"
                      primary
                    ></v-switch>
                  </v-col>-->
                </v-row>
              </v-card-text>
              <!-- <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn text>Cancel</v-btn>
                <v-btn text color="primary">Submit</v-btn>
              </v-card-actions> -->
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>

    <v-footer :inset="footer.inset" app>
      <v-row align="end" style="margin-top: 10px;">
        <v-switch v-model="$vuetify.theme.dark" primary label="Obscuro"></v-switch>
      </v-row>
      <!-- &copy; {{ new Date().getFullYear() }} -->
      <span class="px-4">E. Gonzalez Trujillo</span>
    </v-footer>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  name: "Page",
  data: () => ({
    tales: [],
    activestory: "select a story",
    drawers: ["Default (no property)", "Permanent", "Temporary"],
    primaryDrawer: {
      model: null,
      type: "default (no property)",
      clipped: false,
      floating: false,
      mini: false
    },
    footer: {
      inset: false
    }
  }),
  methods: {
    set: function(text) {
      this.activestory = text;
    },
    getData: function() {
      var url = "https://talesserve.herokuapp.com/tales";

      axios
        .get(url)
        .then(res => {
          this.tales = res.data.found;
          this.tales = this.tales.reverse();
        })
        .then(() => {
          this.activestory =this.tales[0];
        });
    }
  },
  created() {
    this.getData();
    
  }
};
</script>