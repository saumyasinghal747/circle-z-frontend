<template>
<v-container fluid class="mx-0 px-0" fill-height>
  <v-row><v-col cols="12">
      <v-text-field
        label="Search"
	v-model="search"
        prepend-inner-icon="mdi-magnify"
	clearable
        ></v-text-field>
  </v-col></v-row>
<v-col cols="12">
  <v-row style="position: relative" class="mt-0"  v-for="(folder,i) in videos" :key="i">
    <v-col sm="12">
      <v-card flat>
        <v-card-title>{{ folder.folder }}</v-card-title>
        <div>
          <!--- hopefully there is a nice way to hide buttons when unneeded --->
          <v-row  :ref="`folder-${i}-scroll`" class="scrollbox" style="flex-wrap: nowrap;overflow-x:scroll">
            <v-btn :ref="`folder-${i}-left`" onclick="this.parentElement.scrollTo({left:this.parentElement.scrollLeft + this.parentElement.children[2].clientWidth,top: 0, behavior:'smooth'})" style="position: absolute;z-index: 1;top:50%;right:0;transform: translate(-50%, -50%);" fab elevation="2"><v-icon>mdi-chevron-right</v-icon></v-btn>
            <v-btn :ref="`folder-${i}-right`" onclick="this.parentElement.scrollTo({left:this.parentElement.scrollLeft - this.parentElement.children[2].clientWidth,top: 0, behavior:'smooth'})" style="position: absolute;z-index: 1;top:50%;left:0;transform: translate(-50%, -50%);" fab elevation="2"><v-icon>mdi-chevron-left</v-icon></v-btn>

            <v-col cols="12" sm="6" md="3"  v-for="(video,j) in folder.videos.filter( (x) => JSON.stringify(x).toLowerCase().match(search ? search.toLowerCase() : '') )" :key="j">
              <v-card  style="height: 100%" :to="`/videos/${video.video}`">
                <v-img
                  max-height="150"
                  width="100%"
                  :src="`https://d4zgr4aptyy2e.cloudfront.net/${video.video}.0000001.jpg`"
                ></v-img>
                <v-card-title style="font-size:medium" >{{ video.title }}</v-card-title>
                <v-card-subtitle style="font-size: small" v-if="video.date">{{ video.date | moment('MMMM Do YYYY') }}, {{video.date | moment("from", "now") }}</v-card-subtitle>
                <v-card-text class="mt-auto">
                  <person v-for="email in video.authors" :key="email" :userId="emails[email]"/>
                </v-card-text>
              </v-card>
            </v-col>

          </v-row>
        </div>
      </v-card>
      <v-divider class="mt-5"/>
    </v-col>
  </v-row>
</v-col>
</v-container>
</template>

<script>
import { mapActions, mapState } from 'vuex';

export default {
  computed: {
    ...mapState(['videos', 'emails']),
  },

  data() {
    return {
      id: undefined,
      search: '',
      key: 1,
    };
  },

  methods: {
    ...mapActions([
      'getVideos',
    ]),
    checkButtons() {
      /* // eslint-disable-next-line no-unused-vars
      const leftButton = this.$refs[`folder-${i}-left`].$el; // [0];
      console.log(leftButton);
      // eslint-disable-next-line no-unused-vars
      const rightButton = this.$refs[`folder-${i}-right`][0];
      const self = this.$refs[`folder-${i}-scroll`][0];
      if (self.scrollLeft < 5) {
        leftButton.style.display = 'none';
      } */
    },
  },

  mounted() {
    return this.getVideos();
  },

};
</script>
<style>
/* hmm https://stackoverflow.com/questions/9333379/check-if-an-elements-content-is-overflowing */
</style>
