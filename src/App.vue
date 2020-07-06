<template>
  <div
    id="app"
    class="fill"
  >

    <div class="container fill">

      <div class="row">
        <div class="col">
          <h1>{{title}}</h1>
          <p>
            See
            <a href="https://github.com/jbaysolutions/vue-grid-layout/issues/333">Document how to "maximize" a GridItem... · Issue #333 · jbaysolutions/vue-grid-layout</a>
          </p>
        </div>
      </div>

      <div class="row">
        <div class="col">
          This is where my portal parant container lives. If you don't see anything yellow, then all portal content gets teleported
          <!-- <keep-alive> -->
          <portal :to="'grid-item-'+selectedTarget">

            <!-- <keep-alive> -->
            <div class="yellow">Teleported Input Box
              <!-- <keep-alive> -->
              <input width="40">
              <!-- </keep-alive> -->
            </div>
            <!-- </keep-alive> -->
          </portal>
          <!-- </keep-alive> -->

          <div>
            <div
              class="form-group"
              label="Individual radios"
            >
              <input
                class="form-check-input"
                v-model="selectedTarget"
                type="radio"
                id="inlineRadio1"
                name="some-radios"
                value="0"
              > <label
                class="form-check-label"
                for="inlineRadio1"
              >
                Option 0
              </label></div>
            <div
              class="form-group"
              label="Individual radios"
            >
              <input
                class="form-check-input"
                v-model="selectedTarget"
                type="radio"
                id="inlineRadio2"
                name="some-radios"
                value="1"
              > <label
                class="form-check-label"
                for="inlineRadio2"
              >
                Option 1
              </label></div>
            <div
              class="form-group"
              label="Individual radios"
            >
              <input
                class="form-check-input"
                v-model="selectedTarget"
                type="radio"
                id="inlineRadio3"
                name="some-radios"
                value="2"
              > <label
                class="form-check-label"
                for="inlineRadio3"
              >
                Option 2
              </label></div>
            <div
              class="form-group"
              label="Individual radios"
            >
              <input
                class="form-check-input"
                v-model="selectedTarget"
                type="radio"
                id="inlineRadio4"
                name="some-radios"
                value="NAN"
              > <label
                class="form-check-label"
                for="inlineRadio4"
              >
                Unknown target portal
              </label></div>
          </div>

          <div class="mt-3">Selected: <strong>{{ selectedTarget }}</strong></div>
        </div>

      </div>

      <div class="row">
        <div class="d-flex justify-content-center align-items-center">
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

        </div>

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
      </div>

    </div>
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
      selectedTarget: null
    };
  },
  methods: {},
  mounted() {
    this.title = "GridLayout with Vue Portal & Grid-Layout: Test Local State Loss";
  }
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
