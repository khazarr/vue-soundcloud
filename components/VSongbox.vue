<template>
  <div class="col-sm-3">
    <div class="card">
        <!--Card image-->
        <div
          class="img-fluid card-image"
          v-bind:style="{ 'background-image': 'url(' + photo + ')' }">
          <div
            v-if="isSongActive()"
            :style="{ 'width': this.setSongProgress() + '%' }"
            class="progress-wave"></div>
          <img
            class="img-responsive"
            :src="waveimg" />
        </div>
        <!--Card content-->
        <div class="card-body">
            <!--Title-->
            <h4 class="card-title">
              {{ user.permalink }}
              <a :href="user.permalink_url" target="_blank">
                <i class="fa fa-external-link pull-right" aria-hidden="true"></i>
              </a>
            </h4>
            <!--Text-->
            <p class="card-text">{{ title }}</p>
            <div class="row">
              <div class="col-sm-6">
                <a href="#" v-on:click.prevent="toggle" class="btn btn-primary">
                  <i v-if="songStatus" class="fa fa-play" aria-hidden="true"></i>
                  <i v-else class="fa fa-pause" aria-hidden="true"></i>
                </a>
              </div>
              <div class="col-sm-6">
                <p class="duration text-right">
                  {{ displayInMinutes() }}
                </p>
              </div>
            </div>

        </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    description: String,
    photo: String,
    waveimg: String,
    id: Number,
    user: Object,
    duration: Number
  },
  methods: {
    toggle () {
      const song = this.$store.getters.getSongById(this.id)
      const audio = document.querySelector('audio')
      this.togglePlayer(audio)
      this.$store.commit('setCurrentSong', { song, status: this.getSongStatus() })
    },
    setSongProgress () {
      return this.id === this.$store.getters.getCurrentSong.id ? this.$store.getters.getSongProgress : 0
    },
    isSongActive () {
      return this.id === this.$store.getters.getCurrentSong.id
    },
    togglePlayer (audio) {
      if (this.$store.getters.getCurrentSong.id !== undefined) {
        audio.paused ? audio.play() : audio.pause()
      }
    },
    displayInMinutes () {
      let time = this.duration / 1000
      time = parseInt(time).toFixed(0)
      return ~~(time / 60) + ':' + (time % 60 < 10 ? '0' : '') + time % 60
    },
    getSongStatus () {
      return !(this.$store.getters.getCurrentSongStatus && this.isSongActive())
    }
  },
  computed: {
    songStatus () {
      return this.getSongStatus()
    }
  }
}
</script>
