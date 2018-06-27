
/* eslint-disable */

<template>
  <div class="container">
      <wxc-minibar 
                   title="TodoApp"
                   background-color="#009ff0"
                   text-color="#FFFFFF"
                   left-text=" "
                   right-button="add"
                   @wxcMinibarRightButtonClicked="minibarRightButtonClick"
      >
      <wxc-icon 
              name="add"
              slot="right"
              size="small"
      ></wxc-icon>
      </wxc-minibar>
    <list ref="scroller">
        <refresh @refresh="onrefresh" :display="refreshing ? 'show' : 'hide'">
        ...
        </refresh>
        <cell>
          <div class="padding"></div>
        </cell>
        <wxc-cell 
            v-for="(item, index) in items" :key="index"
        >
            <text slot="label">{{index + 1}}.-       </text>
            <text slot="title">{{item}}</text>
            <wxc-icon 
              name="delete"
              size="small"
              @wxcIconClicked="iconClicked(index)"
            ></wxc-icon>
        </wxc-cell>
      </list>
  </div>
</template>

<script>
import Header from "./components/header.vue";
import todo from "./components/todos.vue";
import Footer from "./components/footer.vue";
import data from "../db/MOCK_DATA_100.json"
import { WxcMinibar, WxcCell, WxcIcon , WxcSlideNav} from "weex-ui";
const modal = weex.requireModule("modal");

export default {
  name: "App",
  components: {
    Header,
    Footer,
    todo,
    WxcMinibar,
    WxcCell,
    WxcIcon, 
    WxcSlideNav
  },
  data() {
    return {
      items: new Array(50),
      data
    };
  },
  methods: {
    onrefresh (event) {
      this.refreshing = true
      setTimeout(() => {
        this.refreshing = false
      }, 2000)
    },

    minibarRightButtonClick () {
      modal.prompt({
          message: 'Enter new task...'
        }, value => {
          if ( value.result === 'OK' && value.data.length > 0 ) {
            //this.$store.dispatch('ADD_TODO', { name: value.data } )
            this.items.push(value.data)
          }
        })
    },

    iconClicked(index) {
      this.items = this.data
      this.items.splice(index, 1)
      console.log(this.items);
      console.log(this.data);
    }
  }
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Roboto");
.list-title-text {
  font-family: "Roboto", sans-serif;
  font-size: 27px;
  color: #2c3e50;
  font-weight: bold;
  letter-spacing: 2px;
}
</style>
