<template>
  <div
    id="app"
    class="fill"
  >

    <b-container class="fill">

      <b-row>
        <b-col>
          <h1>Vue-Grid-Layout: Keep State while v-show Toggle</h1>
          <p>

          </p>
        </b-col>
      </b-row>

      <b-row>
        <b-col>
          This is where my portal parant container lives. If you don't see anything yellow, then all portal content gets teleported
          <!-- <keep-alive> -->
          <portal to="grid-item-1">

            <!-- <keep-alive> -->
              <div class="yellow" v-show="isGridShown">

                Teleported Input Box 
                <!-- <keep-alive> -->
                  <input v-model.trim="inputMsg" width="40">
                <!-- </keep-alive> -->
              </div>
            <!-- </keep-alive> -->
          </portal>

          <portal to="dock-item-1">

            <!-- <keep-alive> -->
              <div class="red"  v-show="!isGridShown">Teleported Input Box 
                <!-- <keep-alive> -->
                  That's what appears in the dock for View-Item 1 with Input Msg {{inputMsg}}
                <!-- </keep-alive> -->
              </div>
            <!-- </keep-alive> -->
          </portal>
          <!-- </keep-alive> -->

          <div>
            <b-form-group label="Individual radios">
              <b-form-radio
                v-model="selectedTarget"
                name="some-radios"
                value="Grid"
              >Item 1 in Grid</b-form-radio>
              <b-form-radio
                v-model="selectedTarget"
                name="some-radios"
                value="Dock"
              >Item 1 in Dock</b-form-radio>
            </b-form-group>

            <div class="mt-3">Selected: <strong>{{ selectedTarget }}</strong></div>
          </div>

        </b-col>
      </b-row>

      <b-row>
        <b-col class="d-flex justify-content-center align-items-center">
          <button
            class="btn btn-primary p-2 m-2"
            :disabled="maximizeID"
            @click.prevent="maximizeID=1"
          >Maximize item 1</button>
          <button
            class="btn btn-primary p-2 m-2"
            :disabled="!maximizeID"
            @click.prevent="maximizeID=null"
          >Restore</button>
          Maximized: {{maximizeID || "none"}}

        </b-col>
      </b-row>

      <div class="gridContainer">
        <grid-layout
          :layout="layout"
          :col-num="12"
          :row-height="30"
          :is-draggable="! maximizeID"
          :is-resizable="! maximizeID"
          :is-mirrored="false"
          :vertical-compact="true"
          :margin="[10, 10]"
          :use-css-transforms="true"
          class="gridLayout"
          :class="{maximized: maximizeID}"
        >
          <grid-item
            v-for="item in layout"
            :key="item.i"
            :x="item.x"
            :y="item.y"
            :w="item.w"
            :h="item.h"
            :i="item.i"
            class="gridItem"
            :class="{
                     maximized: maximizeID == item.i,
                     hidden: maximizeID && maximizeID != item.i
                  }"
          >{{item.i}}

            <portal-target
              class="d-flex"
              :name="'grid-item-'+item.i"
            >

            </portal-target>

          </grid-item>
        </grid-layout>
      </div>

      <b-row>
        <b-col class="dock">
          <h1>Dock</h1>
          <portal-target name="dock-item-1"></portal-target>
        </b-col>
      </b-row>
    </b-container>

  </div>
</template>


<script>
import { GridItem, GridLayout } from "vue-grid-layout";

export default {
  components: {
    GridLayout,
    GridItem
  },
  props: {
    value: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      title: "",
      layout: [
        { x: 0, y: 0, w: 2, h: 4, i: "0" },
        { x: 2, y: 0, w: 2, h: 4, i: "1" },
        { x: 4, y: 0, w: 2, h: 4, i: "2" }
      ],
      maximizeID: null,
      show: [1,2,3],
      selectedTarget: "Grid",
      inputMsg: "",
    };
  },

  computed: {
    isGridShown() {
      return (this.selectedTarget === "Grid")
    }
  },
  methods: {},
};

</script>

<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 100vh;
}

.red {
  background-color: rgba(360, 0, 0, 0.3);
}
.green {
  background-color: rgba(0, 360, 0, 0.3);
}
.blue {
  background-color: rgba(0, 0, 360, 0.3);
}

.sized {
  flex: auto;
  height: 300px;
}

.yellow {
  background-color: rgba(100, 100, 0, 0.7);
}

input {
  width: 100px;
}

.gridContainer {
  background-color: #ccc;
  height: 500px;
}
.gridLayout {
  background-color: #eee;
}
.gridLayout.maximized {
  height: 100% !important;
}
.gridItem {
  background-color: gray;
  /* width is omitted from the properties in vue-grid-layout
      and that causes transitions to appear weirdly during maximize */
  -webkit-transition-property: left, top, right, width;
  transition-property: left, top, right, width;
}
.gridItem.hidden {
  display: none;
}
.gridItem.maximized {
  transform: inherit !important;
  height: 100% !important;
  width: 100% !important;
}

.fill {
  min-height: 100%;
  height: 100%;
}
</style>
