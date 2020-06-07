<template>
  <v-layout column justify-center align-center>
    <v-flex xs6>
      <v-btn @click="fetch">Fetch data</v-btn>
      <v-icon v-if="fetching">fas fa-circle-notch fa-spin</v-icon>
    </v-flex>

    <v-flex v-if="fetching" xs12 sm8 md6>
      <v-skeleton-loader
        v-for="(item, i) in 10"
        :key="i"
        class="mx-auto pa-2"
        width="300"
        type="card"
      ></v-skeleton-loader>
    </v-flex>

    <v-flex ref="mylist" xs12 sm8 md6>
      <v-list three-line>
        <v-list-item v-for="(item, i) in items" :key="i">
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
            <v-list-item-subtitle v-text="item.body" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-flex>
  </v-layout>
</template>

<script>
import posts from '../store/posts'
// import photos from '../store/photos';

export default {
  data() {
    return {
      mockItems: posts,
      items: posts
    }
  },

  computed: {
    fetching() {
      return !!this.mockItems.length
    }
  },

  mounted() {
    // hide mock list and show real list
    setTimeout(() => {
      this.$refs.mylist.classList.remove('js-disabled')
      this.mockItems = []
    }, 4000)
  },

  methods: {
    async fetch() {
      if (this.fetching) return

      this.mockItems = Array.from({ length: 100 })
      this.items = []

      const res = await fetch('https://jsonplaceholder.typicode.com/posts')
        .then((response) => response.json())
        .catch(() => Array.from(posts))

      await delay(3000)

      this.items = res
      this.mockItems = []
    }
  }
}

function delay(time = 1000) {
  return new Promise((resolve) => setTimeout(resolve, time))
}
</script>

<style lang="scss">
.js-disabled {
  background: blueviolet;
  height: 0;
  max-height: 0;
  width: 0;
  max-width: 0;
  opacity: 0;
  visibility: hidden;
  overflow: hidden;
}
</style>
