<template>
  <div id="app">
    <div class="card-container">
      <h1 v-if="allLinks.length === 0">No saved links</h1>
      <link-card
        v-else
        v-for="link in allLinks"
        :key="link"
        :url="link"
        @refreshLinks="refreshLinks()"
      ></link-card>
    </div>
    <div
      class="tutorial-overlay"
      ref="tutorial"
      :class="firstTime ? 'active' : ''"
    >
      <div class="tool-tip">
        Click here to add <br />your first link
        <div class="tipIcon-container">
          <svg
            height="150px"
            viewBox="0 0 512 511"
            width="150px"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="m368.128906 512.484375c-5.183594 0-10.417968-1.007813-15.414062-3.082031-15.011719-6.226563-24.707032-20.742188-24.707032-36.976563v-91.921875c-87.617187 0-169.984374-34.117187-231.9375-96.070312-61.953124-61.953125-96.070312-144.324219-96.070312-231.9375v-31.996094c0-11.046875 8.953125-20 20-20s20 8.953125 20 20v31.996094c0 158.808594 129.199219 288.007812 288.003906 288.007812 22.058594 0 40.003906 17.941406 40.003906 40v91.921875.011719c.03125.015625.070313.03125.105469.042969.011719-.015625.027344-.027344.042969-.042969l97.960938-97.753906c3.792968-3.785156 5.882812-8.824219 5.882812-14.179688 0-5.359375-2.089844-10.394531-5.882812-14.179687l-97.960938-97.757813c-.015625-.015625-.03125-.027344-.046875-.042968-.03125.011718-.070313.027343-.101563.042968v.011719 21.921875c0 11.046875-8.957031 20-20 20-11.046874 0-20-8.953125-20-20v-21.921875c0-16.234375 9.695313-30.75 24.707032-36.976563 15.027344-6.234374 32.179687-2.839843 43.695312 8.652344l97.960938 97.753906c11.367187 11.34375 17.628906 26.4375 17.628906 42.496094s-6.261719 31.148438-17.628906 42.492188l-97.960938 97.753906c-7.683594 7.671875-17.882812 11.734375-28.28125 11.734375zm0 0"
            />
          </svg>
        </div>
      </div>
    </div>
    <floating-btn
      @refreshLinks="refreshLinks()"
      @notFirstTime="removeFirstTime()"
      @showNotification="showNotification"
    />
    <notification :data="notification" />
  </div>
</template>

<script>
import "./_assets/style.scss";
import LinkCard from "./components/LinkCard";
import FloatingBtn from "./components/FloatingBtn.vue";
import Notification from "./components/Notification.vue";
export default {
  name: "App",
  components: {
    LinkCard,
    FloatingBtn,
    Notification,
  },
  data() {
    return {
      allLinks: JSON.parse(localStorage.getItem("allLinks"))
        ? JSON.parse(localStorage.getItem("allLinks"))
        : [],
      firstTime: localStorage.getItem("firstTime") ? false : true,
      notification: {
        message: "",
        type: "",
      },
    };
  },

  methods: {
    refreshLinks() {
      this.allLinks = JSON.parse(localStorage.getItem("allLinks"));
    },
    removeFirstTime() {
      localStorage.setItem("firstTime", false);
      this.firstTime = false;
    },
    showNotification(payload) {
      this.notification = payload
      
    },
  },
};
</script>

<style lang="scss" scoped>
</style>
