<template>
<div class="home">
  <section class="image-gallery">
    <div class="image" v-for="item in items" :key="item.id">
      <div class="border">
      <h2>{{item.title}}</h2>
      <img :src="item.path" />
      <p>{{item.description}}</p>
      <p>-{{item.place}}-</p>
      </div>
    </div>
  </section>
</div>
</template>

<script>

import axios from 'axios';

export default {
  name: 'Home',
  data() {
    return {
     items: [],
    }
  },
  created() {
    this.getItems();
  },
  methods: {
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        console.log(error);
      }
    },
  }
}
</script>

<style scoped>
.image .border {
  border: 3px solid #741212;
  max-width: 1000%;
  margin: auto;
  padding: 10px;
}
.image h2 {
  font-style: italic;
}


*,
*:before,
*:after {
  box-sizing: inherit;
}

.image-gallery {
  column-gap: 1.5em;
}

.image {
  margin: 0 0 1.5em;
  display: inline-block;
  width: 100%;
}

.image img {
  width: 100%;
}


@media only screen and (min-width: 1024px) {
  .image-gallery {
    column-count: 4;
  }
}


@media only screen and (max-width: 1023px) and (min-width: 768px) {
  .image-gallery {
    column-count: 3;
  }
}


@media only screen and (max-width: 767px) and (min-width: 540px) {
  .image-gallery {
    column-count: 2;
  }
}
</style>
