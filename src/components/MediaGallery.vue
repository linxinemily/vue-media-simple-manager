<template>
  <div>
    <div class="controllBar">
      <button
        class="btn btn-primary"
        @click="handleMutiFiles = true"
        v-if="!handleMutiFiles"
      >
        批次處理
      </button>
      <button
        class="btn btn-primary"
        @click="cancelMutiFile"
        v-else
      >
        取消
      </button>
      <button
        class="btn btn-warning"
        @click="deleteMutiFiles"
        v-show="handleMutiFiles"
      >
        刪除選取圖片
      </button>
    </div>
    <div class="medias">
      <div
        class="media"
        v-for="media in medias"
        :key="media.id"
        :class="[{ 'ready-to-choose': handleMutiFiles }, {'is-chosen':  mutiFiles.includes(media) && handleMutiFiles}]"
      >
        <div class="media-thumbnail" @click="openDetails(media)">
          <div class="media-thumbnail-inner">
            <div class="media-thumbnail-inner-center">
              <img :src="media.url" :alt="media.alt">
            </div>
          </div>
        </div>
      </div>
    </div>
    <transition name="slide-fade">
      <media-box
        :current-media="currentMedia"
        v-if="showDetails"
        @closeBox="closeBox"
        @goToPrev="goToPrev"
        @goToNext="goToNext"
        @deleteMedia="deleteMedia"
        @updateMedia="updateMedia"
      />
    </transition>
  </div>
</template>

<script>
import MediaBox from './MediaBox.vue'
export default {
  props: {
    medias: {
      type: Array,
      required: true
    }
  },
  components: {
    MediaBox
  },
  data() {
    return {
      showDetails: false,
      showUploader: false,
      currentMedia: {},
      handleMutiFiles: false,
      mutiFiles: []
    }
  },
  computed: {
    currentIndex() {
        let current
        this.medias.forEach((el, index) => {
          if (this.currentMedia === el) {
            current = index
          }
        })
        return current
    }
  },
  methods: {
    openDetails(media) {
      if (this.handleMutiFiles === true) {
        if (this.mutiFiles.length === 0) {
          this.mutiFiles.push(media)
        } else {
          if (!this.mutiFiles.includes(media)) {
            this.mutiFiles.push(media)
          } else {
            this.mutiFiles.splice(this.mutiFiles.indexOf(media), 1)
          }
        }
      } else {
          this.showDetails = true
          this.currentMedia = media
      }
    },
    goToPrev() {
      if (this.currentIndex > 0) {
        this.currentMedia = this.medias[this.currentIndex - 1]
      } else if (this.currentIndex === 0) {
        this.currentMedia = this.medias[this.medias.length - 1]
      }
    },
    goToNext() {
      if (this.currentIndex < this.medias.length - 1) {
        this.currentMedia = this.medias[this.currentIndex + 1]
      } else if(this.currentIndex === this.medias.length - 1) {
        this.currentMedia = this.medias[0]
      }
    },
    deleteMedia() {
      this.$emit('deleteMedia', this.currentMedia)
      this.showDetails = false
      // const attempt = this.currentIndex
      // this.medias.splice(this.currentIndex, 1)
      // if (this.medias[attempt]) {
      //   this.currentMedia = this.medias[attempt]
      // } else {
      //   this.currentMedia = this.medias[0]
      // }
    },
    deleteMutiFiles() {
      this.$emit('deleteMutiMedias', this.mutiFiles)
      this.mutiFiles = []
    },
    updateMedia(item) {
      this.$emit('updateMedia', item)
      this.showDetails = false
    },
    closeBox() {
      this.showDetails = false
    },
    // deleteMutiMedias() {
    //   for(let i = 0; i < this.mutiFiles.length; i++) {
    //     this.medias.splice(this.medias.indexOf(this.mutiFiles[i]), 1)
    //   }
    // },
    cancelMutiFile() {
      this.handleMutiFiles = false
      this.mutiFiles = []
    }
    // updateMedia() {
    //   // call update api
    // }
  }
}
</script>



<style lang="scss">
.btn {
  border-radius: 4px;
  font-size: 14px;
  font-weight: 400;
  padding: 6px 10px;
  background-color: #fff;
  border: 1px solid #e5e5e5;
  display: inline-block;
  margin-bottom: 8px;
  &:focus {
    box-shadow: none;
  }
  &-warning {
    background-color: #ca231e;
    border-color: #be211c;
    color: #fff;
  }
  &-primary {
    background-color: #fafafa;
    border-color: #e5e5e5;
  }
  &[disabled="disabled"] {
    opacity: 0.5;
  }
}
.medias {
  display: grid;
  grid-template-columns: repeat(8, 1fr) ;
  grid-gap: 20px;
}

.media {
  &-thumbnail {
    position: relative;
    box-shadow: inset 0 0 15px rgba(0,0,0,.1), inset 0 0 0 1px rgba(0,0,0,.05);
    background: #eee;
    cursor: pointer;
    &:before {
      content: "";
      display: block;
      padding-top: 100%;
    }
    &-inner {
      overflow: hidden;
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      &-center {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        -webkit-transform: translate(50%,50%);
        transform: translate(50%,50%);
        img {
          position: absolute;
          transform: translate(-50%,-50%);
          top: 0;
          left: 0;
          max-height: 100%;
        }
      }
    }
  }
}

.ready-to-choose {
  opacity: 0.5;
}
.is-chosen {
  box-shadow: 0 0 20px;
  box-shadow: 0 0 0 0.2rem rgba(0,123,255,0.25);
  opacity: 1;
}

.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
