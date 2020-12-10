<template>
  <div class="thumbnail-container">
    <div class="thumbnail">
      <iframe :src="url" frameborder="0"></iframe>
    </div>
    <div class="overlay">
      <p>{{ shortUrl }}</p>
    </div>
    <a :href="url"></a>

    <div class="deleteIcn" @click="deleteClicked">
      <svg
        id="Layer_1"
        enable-background="new 0 0 512 512"
        height="20"
        viewBox="0 0 512 512"
        width="20"
        xmlns="http://www.w3.org/2000/svg"
      >
        <g>
          <path
            d="m424 64h-88v-16c0-26.51-21.49-48-48-48h-64c-26.51 0-48 21.49-48 48v16h-88c-22.091 0-40 17.909-40 40v32c0 8.837 7.163 16 16 16h384c8.837 0 16-7.163 16-16v-32c0-22.091-17.909-40-40-40zm-216-16c0-8.82 7.18-16 16-16h64c8.82 0 16 7.18 16 16v16h-96z"
          />
          <path
            d="m78.364 184c-2.855 0-5.13 2.386-4.994 5.238l13.2 277.042c1.22 25.64 22.28 45.72 47.94 45.72h242.98c25.66 0 46.72-20.08 47.94-45.72l13.2-277.042c.136-2.852-2.139-5.238-4.994-5.238zm241.636 40c0-8.84 7.16-16 16-16s16 7.16 16 16v208c0 8.84-7.16 16-16 16s-16-7.16-16-16zm-80 0c0-8.84 7.16-16 16-16s16 7.16 16 16v208c0 8.84-7.16 16-16 16s-16-7.16-16-16zm-80 0c0-8.84 7.16-16 16-16s16 7.16 16 16v208c0 8.84-7.16 16-16 16s-16-7.16-16-16z"
          />
        </g>
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  name: "LinkCard",
  props: {
    url: String,
  },
  methods: {
    
    deleteClicked(e){
        let parentDiv = e.target;
        while(!parentDiv.classList.contains('thumbnail-container')){
            parentDiv = parentDiv.parentNode;
        }
        parentDiv.classList.add('beforeDelete');
        const url = this.url;
        const allLinks = JSON.parse(localStorage.getItem("allLinks"));
        const indexToDelete = allLinks.indexOf(url);
        allLinks.splice(indexToDelete,1);
        localStorage.setItem("allLinks", JSON.stringify(allLinks));
        setTimeout(()=>{
            this.$emit("refreshLinks");
        },500)
    }
  },
  computed: {
    shortUrl() {
        let shortUrl = this.url.split("://")[1];
        if(shortUrl) shortUrl = shortUrl.split("/")[0];
      return this.url.split("://")[1].split("/")[0];
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../_assets/variables.scss";

.thumbnail-container {
  width: $itemWidth;
  height: $itemHeight;
  overflow: hidden;
  position: relative;
  cursor: pointer;
  overflow: hidden;
  border: 1px solid white;
  transition: all ease-in-out .4s;
  &.beforeDelete{
      width: 0%;
      opacity: 0;
  }
  a {
    display: block;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }
  .overlay {
    display: grid;
    place-items: center;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.411);
    transition: opacity ease-in-out 0.4s;
    p {
      font-size: 1.5rem;
      font-weight: bold;
      color: rgb(252, 252, 240);
      transition: transform ease-in-out 0.3s;
    }
  }

  

  .deleteIcn {
    position: absolute;
    display: grid;
    place-items: center;
    background: white;
    border-radius: 5px;
    border: 1px solid black;
    padding: 0.3rem;
    bottom: 0.5rem;
    right: -5rem;
    transition: all ease-in-out 0.2s;
    &:hover {
      background: red;

      svg {
        fill: white;
      }
    }
  }

  &:hover {
    .overlay {
      opacity: 0;
      p {
        transform: translateY(-300px);
      }
    }

    .deleteIcn {
      right: 0.5rem;
    }
  }
}
.thumbnail {
  position: relative;
  height: 100%;
  iframe {
    width: $iframeWidth;
    height: $iframeHeight;
    -ms-zoom: 0.23;
    -moz-transform: scale(0.23);
    -moz-transform-origin: 0 0;
    -o-transform: scale(0.23);
    -o-transform-origin: 0 0;
    -webkit-transform: scale(0.23);
    -webkit-transform-origin: 0 0;
  }
}
</style>