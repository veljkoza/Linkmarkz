<template>
  <div
    class="notification"
    :class="data.type === 'error' ? 'error' : 'success'"
    ref="notification"
  >
    <p>{{ data.message }}</p>
  </div>
</template>

<script>
export default {
  name: "Notification",
  props: {
    data: Object,
  },
  updated() {
    const classList = this.$refs.notification.classList;
    if (classList.contains("active")) return;

    classList.add("active");

    setTimeout(() => {
      classList.remove("active");
    }, 3000);
  },
};
</script>

<style lang="scss" scoped>
.notification {
  position: absolute;
  display: grid;
  place-items: center;
  right: 1rem;
  bottom: 15%;
  height: 60px;
  width: 200px;
  opacity: 0;
  border-radius: 0px;
  transition: all ease-in-out 0.2s;
  border-radius: 500px;

  p {
    color: white;
  }
  &.error {
    background: red;
  }
  &.sucess{
      background: rgb(64, 202, 94);
  }

  &.active {
    opacity: 1;
  }
}
</style>