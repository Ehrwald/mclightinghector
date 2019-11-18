<template>
  <div id="app">

    <v-app v-bind:dark="dark_theme">
      <v-toolbar color="primary" dark>
        <!-- <v-toolbar-side-icon></v-toolbar-side-icon> -->
        <v-toolbar-title>Hector</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-spacer></v-spacer>
        <span class="group pa-2" v-if="is_connected"><v-icon>wifi</v-icon>&nbsp;<span class="hidden-xs-only">&nbsp;McLighting </span>connected</span>
        <span class="group pa-2" v-else><v-icon>signal_wifi_off</v-icon>&nbsp;&nbsp;Hector disconnected</span>
        <span v-if="num_additional_connections > 0">+{{ num_additional_connections }}</span>
        <span class="group ml-2" v-if="connection_failed" @click="ws_reconnect()"><v-btn color="secondary" ><v-icon>autorenew</v-icon>&nbsp;Reconnect</v-btn></span>
      </v-toolbar>
        <div class="container-fluid">
            <div class="d-sm-flex justify-content-between align-items-center mb-4"></div>
            <div class="row">
                <div class="col">
                    <div class="row">
                        <div class="col-md-6 col-lg-6 mb-4">
                            <div class="card text-white bg-primary shadow">
                                <a href="#content"><button class="card-body" v-on:click="changePart('head')" >
                                    <p class="m-0">Tete</p>
                                    <p class="text-white-50 small m-0"><img src="./assets/HectorFace.png" class="imageface"></p>
                                </button></a>
                            </div>
                        </div>
                        <div class="col-md-6 col-lg-6 mb-4">
                            <div class="card text-white bg-success shadow">
                                <a href="#content"><button class="card-body" v-on:click="changePart('right_front')" >
                                    <p class="m-0">Cote devant droit</p>
                                    <p class="text-white-50 small m-0"><img src="./assets/HectorCoteDroitAvant.png" class="imagegauche"></p>

                                </button></a>
                            </div>
                        </div>
                        <div class="col-md-6 col-lg-6 mb-4">
                            <div class="card text-white bg-info shadow">
                                <a href="#content"> <button class="card-body" v-on:click="changePart('right_middle')">
                                    <p class="m-0">Cote milieu droit</p>
                                    <p class="text-white-50 small m-0"><img src="./assets/HectorCoteDroitMilieu.png" class="imagedroit"></p>
                                </button></a>
                            </div>
                        </div>
                        <div class="col-md-6 col-lg-6 mb-4">
                            <div class="card text-white bg-info shadow">
                                <a href="#content"> <button class="card-body" v-on:click="changePart('right_back')">
                                    <p class="m-0">Derriere droit</p>
                                    <p class="text-white-50 small m-0"><img src="./assets/HectorCoteDroitArriere.png" class="imagedroit"></p>
                                </button></a>
                            </div>
                        </div>
                        <div class="col-md-6 col-lg-6 mb-4">
                        <div class="card text-white bg-success shadow">
                            <a href="#content"><button class="card-body" v-on:click="changePart('left_front')">
                                <p class="m-0">Devant gauche </p>
                                <p class="text-white-50 small m-0"><img src="./assets/HectorCoteGaucheAvant.png" class="imagedroit"></p>
                            </button></a>
                        </div>
                    </div>
                        <div class="col-md-6 col-lg-6 mb-4">
                            <div class="card text-white bg-warning shadow">
                                <a href="#content">    <button class="card-body" v-on:click="changePart('left_middle')">
                                    <p class="m-0">Milieu gauche</p>
                                    <p class="text-white-50 small m-0"><img src="./assets/HectorCoteGaucheMilieu.png" class="imagedroit"></p>
                                </button></a>
                            </div>
                        </div>
                        <div class="col-md-6 col-lg-6 mb-4" >
                            <div class="card text-white bg-info shadow" >
                                <a href="#content"> <button class="card-body" v-on:click="changePart('left_back')" >
                                    <p class="m-0">Derriere gauche</p>
                                    <p class="text-white-50 small m-0"><img src="./assets/HectorCoteGaucheArriere.png" class="imagedroit"></p>
                                </button></a>
                            </div>
                        </div>
                        <div class="col-md-6 col-lg-6 mb-4">
                            <div class="card text-white bg-primary shadow">
                                <a href="#content">   <button class="card-body" v-on:click="changePart('all')">
                                    <p class="m-0">Tout </p>
                                    <p class="text-white-50 small m-0"><img src="./assets/HectorCote.png" class="imagedroit"></p>
                                </button> </a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <v-card
    class="mx-auto"
    max-width="344"
    outlined
  >
    <v-list-item three-line>
      <v-list-item-content>
        <div class="overline mb-4">OVERLINE</div>
        <v-list-item-title class="headline mb-1">Headline 5</v-list-item-title>
        <v-list-item-subtitle>Greyhound divisely hello coldly fonwderfully</v-list-item-subtitle>
      </v-list-item-content>

      <v-list-item-avatar
        tile
        size="80"
        color="grey"
      ></v-list-item-avatar>
    </v-list-item>

    <v-card-actions>
      <v-btn text>Button</v-btn>
      <v-btn text>Button</v-btn>
    </v-card-actions>
  </v-card>

      <Loader  v-if="loader_state > 0" :state="loader_state" max_states="4" name="content"/>

      <!--<v-content v-if="loader_state === 0"  class="mx-auto">-->
        <v-card>
          <!-- Color selector -->
          <v-layout row wrap>
            <v-flex xs12 sm5>
              <ColorPicker id="content" v-on:selected="onColorSelected" :prop_color="color" :prop_type="picker_type"/>
            </v-flex>
            <v-flex xs12 sm7>
              <v-card-text>
                <v-slider v-model="color.r" label="R" :max="255" v-on:input="set_color" color="red" persistent-hint :hint="(color.r || 0)+''" :disabled="auto_mode"></v-slider>
                <v-slider v-model="color.g" label="G" :max="255" v-on:input="set_color" color="green" persistent-hint :hint="(color.g || 0)+''" :disabled="auto_mode"></v-slider>
                <v-slider v-model="color.b" label="B" :max="255" v-on:input="set_color" color="blue" persistent-hint :hint="(color.b || 0)+''" :disabled="auto_mode"></v-slider>
                <v-slider v-model="speed" prepend-icon="slow_motion_video" :max="255" v-on:input="set_speed" persistent-hint :hint="(speed || 0)+''" :disabled="auto_mode"></v-slider>
                <v-switch v-model="auto_mode" prepend-icon="play_circle_outline" :label="`AUTO: ${(auto_mode) ? 'ON' : 'OFF'}`" color="primary"></v-switch>
              </v-card-text>
            </v-flex>
          </v-layout>

          <!-- Mode list -->
          <v-container pb-5>
            <v-layout row wrap>
              <v-flex xs12 sm6 lg4 xl3 v-for="(mode, index) in modes" :key="mode.id" v-if="edit_mode || !mode.hidden">
                <span v-if="edit_mode">
                  <v-icon
                    color="grey lighten-1"
                    @click="toggle(mode, index)"
                    v-if="mode.hidden === true"
                    >star_border</v-icon>
                  <v-icon
                    color="yellow darken-2"
                    @click="toggle(mode, index)"
                    v-else>star</v-icon>
                </span>

                <v-btn :id="mode.id" v-on:click="set_mode(mode.id)" :disabled="auto_mode" class="elevation-6 white--text" v-bind:class="[modeIsActive(mode) ? 'red' : 'blue']">{{ mode.title }} ({{ mode.id }})</v-btn>
              </v-flex>
            </v-layout>
          </v-container>

          <!-- Settings button -->
          <v-fab-transition>
            <v-btn
              v-show="!edit_mode"
              transition="scale-transition"
              @click.native.stop="edit_mode = !edit_mode"
              color="pink"
              dark
              absolute
              bottom
              right
              fab
              hover>
              <v-icon>settings</v-icon>
            </v-btn>
          </v-fab-transition>
        </v-card>

        <!-- Settings tab -->
        <v-card row wrap v-if="edit_mode" id="settings">
          <v-layout row wrap class="pa-3 my-2">
            <v-flex xs12 sm6>
              <p>Dark mode:</p>
              <v-switch :label="`${(dark_theme) ? 'ON' : 'OFF'} `" v-model="dark_theme" color="primary"></v-switch>
            </v-flex>
            <v-flex xs12 sm6>
              <p>Select color picker:</p>
              <v-btn-toggle v-model="picker_type" mandatory>
                <v-btn flat value="circle">
                  Circle
                </v-btn>
                <v-btn flat value="wheel">
                  Wheel
                </v-btn>
              </v-btn-toggle>
            </v-flex>
            <v-flex xs12>
              <v-text-field v-model="slave_nodes" label="Additional McLighting Slave Nodes (experimental)"></v-text-field>
            </v-flex>
            <v-flex xs12>
              <v-btn block color="green" dark class="elevation-6 mb-2" @click="saveSettings()"><v-icon>save</v-icon> Save settings</v-btn>
            </v-flex>
          </v-layout>
        </v-card>

        <v-footer color="primary white--text" class="pa-3">
          <div><a href="http://www.0.4.uha.fr/" target="_blank" class="white--text">Projet by : Luc Ratelli, Florian Biller, Franck Hubschwerle, Thomas Fritsch et Corentin Jacob.</a> - &copy; {{ new Date().getFullYear() }} <img style="max-width: 10%" src="./assets/uha4.0-logo.png" class="imagedroit"></div>
          <v-spacer></v-spacer>
        </v-footer>
      </v-content>

      <!-- Snackbar -->
      <v-snackbar
        :timeout="snackbar_timeout"
        top
        :color="snackbar_color"
        v-model="snackbar"
      >
        {{ snackbar_text }}
      </v-snackbar>
    </v-app>

  </div>
</template>

<script>
/* eslint-disable */
import ColorPicker from "./components/ColorPicker";
import Loader from "./components/Loader";
import ReconnectingWebSocket  from 'reconnecting-websocket';  // https://github.com/pladaria/reconnecting-websocket

let  arrayTests = [
      ["head", [ "192.168.100.205","192.168.100.206", "192.168.100.204"]],
      ["right_front", [ "192.168.100.201","192.168.100.202", "192.168.100.203"]],
      ["right_middle", [ "192.168.100.207","192.168.100.208", "192.168.100.209"]],
      ["right_back", [ "192.168.100.210"]]];

let arrayProd =[ ["head", [ "192.168.100.147", "192.168.100.148", "192.168.100.149", "192.168.100.150"]],
  ["right_front",["192.168.100.136", "192.168.100.137", "192.168.100.139", "192.168.100.141", "192.168.100.142", "192.168.100.143","192.168.100.144", "192.168.100.145", "192.168.100.146"]],
  ["right_middle", ["192.168.100.130", "192.168.100.131", "192.168.100.132", "192.168.100.133", "192.168.100.134", "192.168.100.135", "192.168.100.138", "192.168.100.140"]],
  ["right_back", ["192.168.100.125", "192.168.100.126", "192.168.100.127", "192.168.100.128", "192.168.100.129"]],
  ["left_front", ["192.168.100.101", "192.168.100.102", "192.168.100.103", "192.168.100.104", "192.168.100.105", "192.168.100.106", "192.168.100.107", "192.168.100.108"]],
  ["left_middle", ["192.168.100.109", "192.168.100.110", "192.168.100.111", "192.168.100.112", "192.168.100.113", "192.168.100.114", "192.168.100.115", "192.168.100.116"]],
  ["left_back", ["192.168.100.117", "192.168.100.118", "192.168.100.119", "192.168.100.120", "192.168.100.121", "192.168.100.122", "192.168.100.123", "192.168.100.124"]],
  ["all", [ "192.168.100.147", "192.168.100.148", "192.168.100.149", "192.168.100.150", "192.168.100.136", "192.168.100.137", "192.168.100.139", "192.168.100.141", "192.168.100.142", "192.168.100.143","192.168.100.144", "192.168.100.145", "192.168.100.146","192.168.100.130", "192.168.100.131", "192.168.100.132", "192.168.100.133", "192.168.100.134", "192.168.100.135", "192.168.100.138", "192.168.100.140","192.168.100.125", "192.168.100.126", "192.168.100.127", "192.168.100.128", "192.168.100.129" ,"192.168.100.101", "192.168.100.102", "192.168.100.103", "192.168.100.104", "192.168.100.105", "192.168.100.106", "192.168.100.107", "192.168.100.108","192.168.100.109", "192.168.100.110", "192.168.100.111", "192.168.100.112", "192.168.100.113", "192.168.100.114", "192.168.100.115", "192.168.100.116","192.168.100.117", "192.168.100.118", "192.168.100.119", "192.168.100.120", "192.168.100.121", "192.168.100.122", "192.168.100.123", "192.168.100.124" ]]
];

let array = arrayProd;//TODO modify for prod

var host = "localhost";
if (host === "localhost") {
  host = array[0][1][0];
}
const ws_url = "ws://" + host + ":81";

let ws_options = {
  connectionTimeout: 1000,
  maxRetries: 2,
  reconnectionDelayGrowFactor: 2,
  debug: true
};

export default {
  name: "McLightingUI",

  components: {
    ColorPicker,
    Loader
  },

  data: () => ({
    color: {r:0, g:0, b:0, hex:"000000"},
    brightness: 192,
    speed: 192,
    modes: [],//[{ title: "OFF", id: "off" }, { title: "TV", id: "tv" }], // comment because it's already define somewhere
    connection: null,
    connection_failed: false,
    additional_connections: [],
    connectionsMap: new Map(), //map to stock all node connections
    num_additional_connections: 0,
    is_connected: false,
    dark_theme: false,
    ws2812fx_mode: null,
    auto_mode: false,
    settings: {},
    edit_mode: false,
    picker_type: "circle",
    slave_nodes: "",
    snackbar: false,
    snackbar_text: "",
    snackbar_color: "info",
    snackbar_timeout: 2000,
    loader_state: 1,
    part: "head" //part selected in vue to modification
  }),

  watch: {
    auto_mode: {
      handler: function(val, oldVal) {
        this.toggleAutoplay(val);
      }
    }
  },

  methods: {
    getModes() {
      var that = this;
      this.ws_connect();
      console.log("tentative de connexion " + "//" + array[0][1][0] + "/get_modes");
      this.$http.get("//" + array[0][1][0] + "/get_modes").then(data => {
        console.log("finish load mode");
        // console.log("Getting modes list via REST:", data);
        data.body.forEach(item => {
          if (item.name && item.name.length > 0) {
            that.modes.push({ title: item.name, id: item.mode });
          }
        });
        connectAllNodes();
      }, response => {
        console.error("ERROR loading modes", response);
        this.showSnackbar("Error loading animation modes, please try again...", "error", 5000);
      });
    },
    /* getModes() {

      this.$http.get("//" + host + "/get_modes").then(data => {
        // console.log("Getting modes list via REST:", data);
        data.body.forEach(item => {
          if (item.name && item.name.length > 0) {
            that.modes.push({ title: item.name, id: item.mode });
          }
        });
        this.readSettings();
      }, response => {
        console.error("ERROR loading modes", response);
        this.showSnackbar("Error loading animation modes, please try again...", "error", 5000);
      });
    } ,*/
    modeIsActive(mode) {
      return mode.id === this.ws2812fx_mode;
    },

    showSnackbar(message, color, timeout) {
      this.snackbar_text = message;
      this.snackbar_color = color;
      this.snackbar_timeout = timeout;
      this.snackbar = true;
    },

    readSettings() {
      let that = this;
      this.loader_state = 2;
      this.$http.get("//" + host + "/uistate.json").then(data => {
        console.log("readSettings()", data.body);
        this.settings = data.body || {};
        this.applySettings();
        this.connectAdditionalNodes();
        this.ws_connect();
      }, response => {
        console.warn("ERROR loading settings", response);
        this.ws_connect();
      });
    },
    applySettings() {
      this.modes.forEach((mode, index) => {
        if (this.settings.visibility.includes(mode.id)) {
          mode.hidden = true;
          this.$set(this.modes, index, mode);
        }
      });
      this.dark_theme = this.settings.dark_theme || false;
      this.picker_type = this.settings.picker_type || "circle";
      this.slave_nodes = this.settings.slave_nodes || "";
    },
    saveSettings() {
      var visibility = this.modes.map(mode => {
        if (mode.hidden) {
          return mode.id;
        }
      }).filter((x) => {return x >= 0});
      this.settings.visibility = visibility;
      this.settings.dark_theme = this.dark_theme;
      this.settings.picker_type = this.picker_type;
      this.settings.slave_nodes = this.slave_nodes;

      var formData = new FormData();
      var blob = new Blob([JSON.stringify(this.settings)], {type: 'application/json'});
      formData.append('settings', blob, '/uistate.json');

      this.$http.post("//" + host + "/edit", formData).then(data => {
        console.log("SUCCESS saveSettings()", data, this.settings);
        this.showSnackbar("Settings saved", "success", 1500);
        this.connectAdditionalNodes();
        this.edit_mode = false;
      }, err => {
        console.error("ERROR saveSettings()", err);
        this.showSnackbar("Error saving settings, please try again...", "error", 5000);
      });
    },

    toggle(mode, index) {
      mode.hidden = !mode.hidden;
      this.$set(this.modes, index, mode);
    },
    toggleAutoplay(mode) {
      var message = (mode) ? "start" : "stop";
      this.ws_send(message);
      if (!mode) {
        this.ws_send("$");
      }
    },

    /**
     * Connect to all ESP
     * **/
    connectAllNodes(){
      var map = new Map(array);
      for(var [part, ipArray] of map.entries()){
        console.log("ipArray for key "+ part + ipArray);
        ipArray.forEach((ip) => {
          if (ip !== host){
            console.log("connect" + ip);
            this.ws_connectWithAttributes(ip, part);
          }
        });
      }
    },

    disconnectAllAdditionalNodes() {
      // Close potentially open connections
      this.additional_connections.forEach((conn) => {
        conn.close(1000, "Manually closed::disconnectAllAdditionalNode()", {keepClosed: true});
      });
      this.additional_connections = [];
      this.num_additional_connections = 0;
    },
    connectAdditionalNodes() {
      var that = this;
      this.disconnectAllAdditionalNodes();

      // Connect to the configured nodes
      let nodes = this.slave_nodes.split(";");
      nodes.forEach((host) => {
        host = host.trim();
        if (host !== "") {
          let conn = new ReconnectingWebSocket(ws_url, "arduino", ws_options);
          console.log("Connecting to additional node", host);
          that.additional_connections.push(conn);

          conn.onopen = () => {
            console.log("Connected to additional node", host);
            this.num_additional_connections++;
          };
          conn.onclose = () => {
            console.log("Disconnected to additional node", host);
            that.num_additional_connections--;
          };
          conn.onerror = () => {
            console.log("Error on additional node", host);
          };
        }
      });
    },

    /**
     * Reconnect all nodes where click in reconnect button
     * **/
    ws_reconnect() {
      this.connection.reconnect();
      for (var connect of this.connectionsMap.entries()){
        console.log("reconnect "+ connect);
        connect.reconnect;
      }
      this.connection_failed = false;
    },

    /**
     * Ugly function, from copy/paste of ws_connect to connect one of esp from there ip
     * **/
    ws_connectWithAttributes(ip, part) {
      var that = this;
      var connection = new ReconnectingWebSocket("ws://"+ip+":81", "arduino", ws_options);

      // When the connection is open, send some data to the server
      connection.onopen = function() {
        console.log("WebSocket open ", that.connectionsMap);
        var arrayOfPart = that.connectionsMap.get(part);
        if (arrayOfPart === undefined){
          arrayOfPart = new Array();
        }
        arrayOfPart.push(connection);
        that.connectionsMap.set(part, arrayOfPart);
      };

      // When the connection is open, send some data to the server
      connection.onclose = function() {
        console.log("WebSocket closed");
        that.is_connected = false;
      };

      // Log errors
      connection.onerror = function(error) {
        console.error("WebSocket error", error);
        that.is_connected = false;

        if (that.connection.retryCount >= ws_options.maxRetries) {
          that.connection_failed = true;
          that.showSnackbar("Could not connect to McLighting. Tried " + ws_options.maxRetries + " times. Please use the RECONNECT button to try again.", "error", 15000);
        }
      };

      // Log messages from the server
      connection.onmessage = function(e) {
        console.log("WebSocket from server:", e.data);
        try {
          var res = JSON.parse(e.data);
          // console.log("res", res);
          if (res && res.ws2812fx_mode) {
            that.ws2812fx_mode = res.ws2812fx_mode;
          }
          if (res && res.speed) {
            that.speed = res.speed;
          }
          if (res && res.brightness) {
            that.brightness = res.brightness;
          }
          if (res && res.color) {
            that.color.r = res.color[0];
            that.color.g = res.color[1];
            that.color.b = res.color[2];
            that.color.hex = that.rgbToHex([that.color.r, that.color.g, that.color.b]);
          }
        } catch (e) {}
      };
    },

    /**
     * Connect all nodes
     * **/
    ws_connect() {
      var that = this;
      this.connection = new ReconnectingWebSocket(ws_url, "arduino", ws_options);
      this.connectAllNodes();
      this.loader_state = 3;

      // When the connection is open, send some data to the server
      this.connection.onopen = function() {
        console.log("WebSocket open");
        that.is_connected = true;
        that.loader_state = 4;
        setTimeout(() => {
          that.loader_state = 0;
          that.ws_send("$");
        }, 500);
      };

      // When the connection is open, send some data to the server
      this.connection.onclose = function() {
        console.log("WebSocket closed");
        that.is_connected = false;
      };

      // Log errors
      this.connection.onerror = function(error) {
        console.error("WebSocket error", error);
        that.is_connected = false;

        if (that.connection.retryCount >= ws_options.maxRetries) {
          that.connection_failed = true;
          that.showSnackbar("Could not connect to McLighting. Tried " + ws_options.maxRetries + " times. Please use the RECONNECT button to try again.", "error", 15000);
        }
      };

      // Log messages from the server
      this.connection.onmessage = function(e) {
        console.log("WebSocket from server:", e.data);
        try {
          var res = JSON.parse(e.data);
          // console.log("res", res);
          if (res && res.ws2812fx_mode) {
            that.ws2812fx_mode = res.ws2812fx_mode;
          }
          if (res && res.speed) {
            that.speed = res.speed;
          }
          if (res && res.brightness) {
            that.brightness = res.brightness;
          }
          if (res && res.color) {
            that.color.r = res.color[0];
            that.color.g = res.color[1];
            that.color.b = res.color[2];
            that.color.hex = that.rgbToHex([that.color.r, that.color.g, that.color.b]);
          }
        } catch (e) {}
      };
    },

    /**
     * It's modified function from mcLightning
     * it's send message of all part of body
     * **/
    ws_send(message) {
      var map = new Map(array);
      console.log("WS send: ", message, this.connectionsMap.get(this.part));
      this.connectionsMap.get(this.part).forEach((conn) => {
        console.log("WS send: ", message);
        conn.send(message);
      })
    },

    set_mode(mode_id) {
      this.ws2812fx_mode = mode_id;
      if (Number.isInteger(mode_id)) {
        this.ws_send("/" + mode_id);
      } else {
        // For named modes
        this.ws_send("=" + mode_id);
      }
    },
    set_speed(speed) {
      this.ws_send("?" + speed);
    },
    set_brightness(brightness) {
      this.ws_send("%" + brightness);
    },
    set_color() {
      this.ws_send(
        "#" + this.rgbToHex([this.color.r, this.color.g, this.color.b])
      );
    },

    componentToHex(c) {
      return ("0" + Number(c).toString(16)).slice(-2).toUpperCase();
    },

      changePart(part){
          this.part = part;
          console.log(part);

      },
    rgbToHex(rgb) {
      return (
        this.componentToHex(rgb[0]) +
        this.componentToHex(rgb[1]) +
        this.componentToHex(rgb[2])
      );
    },

    onColorSelected(color) {
      this.color = color;
      this.set_color();
    }
  },

    mounted() {
      this.getModes();
    }
  }

</script>

<style>

    a{
        color: white !important;
    }



    .imagedroit{
        height: 30%;
        width: 30%;
    }
    .imageface{

        height: 40%;
        width: 16%;
    }
    .imagegauche{
        height: 30%;
        width: 30%;
        rotation: 90deg;
    }
    .navbar-light .navbar-brand {
        margin-left: 15px;
        color: #2196f3 !important;
        font-weight: bolder;
    }
  #content {
    max-width: 1380px;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  /* fallback */
  @font-face {
    font-family: 'Material Icons';
    font-style: normal;
    font-weight: 400;
    /* src: url(https://fonts.gstatic.com/s/materialicons/v29/2fcrYFNaTjcS6g4U3t-Y5ZjZjT5FdEJ140U2DJYC3mY.woff2) format('woff2'); */
    src: url(../node_modules/material-design-icons-iconfont/dist/fonts/MaterialIcons-Regular.woff2) format('woff2');
  }

  .material-icons {
    font-family: 'Material Icons';
    font-weight: normal;
    font-style: normal;
    font-size: 24px;
    line-height: 1;
    letter-spacing: normal;
    text-transform: none;
    display: inline-block;
    white-space: nowrap;
    word-wrap: normal;
    direction: ltr;
    -webkit-font-feature-settings: 'liga';
    -webkit-font-smoothing: antialiased;
  }

  .btn__content {
    height: 100% !important;
  }
</style>
