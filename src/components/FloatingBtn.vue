<template>
  <div class="floatingBtn-container">
    <div class="input-container">
      <input
        type="text"
        ref="input"
        placeholder="Enter url here..."
        :class="inputActive ? 'active' : ''"
        v-model="inputValue"
      />
    </div>
    <div
      class="floatingBtn"
      @click="btnClicked"
      ref="fltBtn"
      :class="inputActive ? 'check' : ''"
    >
      <span></span><span></span>
    </div>
  </div>
</template>

<script>
export default {
  name: "FloatingBtn",
  data() {
    return {
      emptyInput: false,
      inputActive: false,
      inputValue: "",
      allLinks: localStorage.getItem("allLinks")
        ? JSON.parse(localStorage.getItem("allLinks"))
        : [],
    };
  },
  methods: {
    btnClicked() {
      this.$emit("notFirstTime");

      this.allLinks = localStorage.getItem("allLinks")
        ? JSON.parse(localStorage.getItem("allLinks"))
        : [];
      const inputClassList = this.$refs.input.classList;
      this.inputActive = true;

      if (this.inputValue) {
        if (
          !this.inputValue.includes("http://") &&
          !this.inputValue.includes("https://")
        ) {
          const payload = {
            message: "Invalid url format!",
            type: "error",
          };
          this.$emit("showNotification", payload);
          return;
        }
        this.inputActive = false;
        this.addLink(this.inputValue);
        this.inputValue = "";
        inputClassList.remove("active");
      } else {
        this.inputActive = true;
        if (!inputClassList.contains("emptyInput")) {
          inputClassList.add("emptyInput");
        }
        setTimeout(() => {
          inputClassList.remove("emptyInput");
        }, 300);
        return;
      }
      inputClassList.add("active");
    },
    addLink(url) {
      this.allLinks.push(url);
      const allLinks = JSON.stringify(this.allLinks);
      localStorage.setItem("allLinks", allLinks);
      this.$emit("refreshLinks");
    },
  },
};
</script>

<style lang="scss" scoped>
@keyframes shake {
  10%,
  90% {
    transform: translateX(-1px);
  }

  20%,
  80% {
    transform: translateX(2px);
  }

  30%,
  50%,
  70% {
    transform: translateX(-4px);
  }

  40%,
  60% {
    transform: translateX(4px);
  }
}
.floatingBtn-container {
  display: flex;
  position: fixed;
  justify-content: space-between;
  width: 70%;
  right: 1%;
  bottom: 1rem;
  height: 70px;
  overflow: hidden;

  .input-container {
    position: relative;
    right: -3.5rem;
    width: 100%;
    height: 100%;
    transition: ease-out 0.6s;
    .emptyInput {
      animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
    }
    input {
      position: absolute;
      border-radius: 500px;
      right: 0;
      bottom: 0;
      opacity: 0;
      width: 0%;
      height: 100%;
      transition: ease-in-out 0.3s;
      padding-left: 3rem;
      font-size: 1.5rem;
      outline: none;
      &.error {
        border: 5px solid red;
      }
      &.active {
        width: 100%;
        opacity: 0.7;
      }
    }
  }
  .floatingBtn {
    border-radius: 50%;
    height: 100%;
    width: 70px;
    background: rgb(56, 192, 129);
    z-index: 10;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    cursor: pointer;
    &:hover {
      background: rgb(63, 204, 138);
    }
    span {
      height: 5px;
      width: 60%;
      background-color: #202c39;
      transition: all ease-in 0.3s;
      position: absolute;
      &:first-child {
        transform: rotate(90deg);
        right: 0.8rem;
      }
      &:last-child {
        bottom: 2rem;
        left: 0.8rem;
      }
    }

    &.check {
      span {
        background-color: white;
        border-radius: 10px;

        &:first-child {
          transform: rotate(110deg);
          right: 0.5rem;
        }
        &:last-child {
          width: 30%;
          bottom: 1.35rem;
          left: 1.05rem;
          transform: rotate(50deg);
        }
      }
    }
  }
}
</style>