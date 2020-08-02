<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <h2>Yuri Viewer</h2>
      </div>

      <v-spacer />

      <v-tooltip bottom>
        <template v-slot:activator="{ on, attrs }">
           <v-btn
            icon
            v-bind="attrs"
            v-on="on"
            link
            href="https://github.com/NotZoeyDev/CuteYuriBot"
            target="blank"
          >
            <v-icon>mdi-code-tags</v-icon>
          </v-btn>
        </template>
        <span>Bot source code!</span>
      </v-tooltip>

      <v-tooltip bottom>
        <template v-slot:activator="{ on, attrs }">
           <v-btn
            icon
            v-bind="attrs"
            v-on="on"
            link
            href="https://twitter.com/CuteYuriBot"
            target="blank"
          >
            <v-icon>mdi-twitter</v-icon>
          </v-btn>
        </template>
        <span>The Twitter account!</span>
      </v-tooltip>
    </v-app-bar>

    <v-main>
      <v-container
        class="fill-height"
      >
        <v-row
          justify="center"
        >
          <v-col
            lg=6
            xs=12
            
          >
            <v-card
              elevation=4
            >
              <v-toolbar color="primary">
                <v-toolbar-title class="white--text">{{ posts[post].title }}</v-toolbar-title>
              </v-toolbar>
              <v-carousel
                hide-delimiters
                @change="setId"
                width="auto"
              >
                <v-carousel-item
                  v-for="(post,i) in posts"
                  :key="i"
                  :src="post.img_url"
                  contain
                >
                </v-carousel-item>
              </v-carousel>

              <v-card-actions>
                <v-btn
                  color="primary"
                  @click="showOriginal"
                >
                  View original
                </v-btn>

                <v-btn
                  v-if="posts[post].source_url != ''"
                  color="primary"
                  @click="showSource"
                >
                  View source
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',

  data: () => ({
    posts: [],
    post: 0,
    post_id: 0,
    loaded: false
  }),

  async mounted() {
    axios.get("https://yuri-backend.panties.moe/posts.json").then((response) => {
      this.posts = response.data.reverse();
      this.post_id = this.posts[0].id;
    });
  },

  methods: {
    setId(value) {
      this.post = value;
      
    },

    showOriginal() {
      window.open(`https://reddit.com/${this.posts[this.post].post_url}`, '_blank');
    },

    showSource() {
      window.open(this.posts[this.post].source_url, '_blank');
    }
  }
};
</script>
