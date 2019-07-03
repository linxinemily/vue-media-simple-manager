<template>
  <div>
    <button
      class="btn btn-primary"
      @click="showUploader = !showUploader"
    >
      新增圖片
    </button>
    <div v-if="showUploader" class="outer" @click="$refs.input.click()">
      <input
        ref="input"
        class="click_zone"
        type="file"
        accept="image/*"
        multiple
        @change="handleUpload($event)"
        style="opacity: 0;">
      <div
        class="drop_zone"
        @drop.prevent="handleUpload($event)"
        @dragover.prevent="handleDragover"
        @dragleave.prevent="handleDragleave"
        :class="{ dragover: isDrag }"
      >
      </div>
      <div v-if="readyToUpload">
        <div class="text">{{ loadingStatus }}</div>
        <div>{{ percentage }} %</div>
        <div>
          <progress max="100" :value.prop="percentage"></progress>
        </div>
        <div
          class="btn btn-primary"
          v-show="successUpload"
        >
          繼續上傳
        </div>
      </div>
      <div v-else class="text">Drag and drop or click to upload</div>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    apiUrl: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      file: null,
      isDrag: false,
      showUploader: false,
      readyToUpload: false,
      successUpload: false,
      loadingStatus: '',
      percentage: 0
    }
  },
  methods: {
    handleUpload (e) {
      this.readyToUpload = true
      this.isDrag = false
      this.percentage = 0
      let files
      if (e.target.files) {
        files = e.target.files
      } else {
        files = e.dataTransfer.files
      }

      for (let i =0; i < files.length; i++) {
        const fd = new FormData()
        fd.append('image', files[i])
        fd.append('alt', 'alt')
        fd.append('title', 'title')

        const upload = new Promise((resolve, reject) => {
          let xhr = new XMLHttpRequest()
          xhr.upload.addEventListener('progress', e => {
            let percent = (event.loaded / event.total) * 100
            this.percentage = Math.round(percent)
          }, false)
          xhr.upload.addEventListener('load', e => {
            this.loadingStatus = 'Done!'
          }, false)
          xhr.upload.addEventListener('error', e => {
            console.log(e)
          }, false)
          xhr.open('POST', this.apiUrl)
          xhr.send(fd)
          xhr.onreadystatechange = function () {
          if (this.readyState == XMLHttpRequest.DONE) {
              if(this.status === 200 || this.status === 201) {
                  const jsonObject = JSON.parse(this.responseText);
                  resolve(jsonObject)
              } else {
                  reject('failed to upload')
              }
            }
          }
        })

        upload.then(data => {
          this.$emit('uploadMedia', data)
          this.successUpload = true
        })
        .catch(e => {
          alert(e)
          this.readyToUpload = false
        })
      }
    },
    handleDragover (e) {
      this.isDrag = true
    },
    handleDragleave (e) {
      this.isDrag = false
    }
  }
}
</script>

<style lang="scss" scoped>
.outer {
  min-height: 300px;
  border: 1px dashed #eee;
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.click_zone, .drop_zone {
  width: 100%;
  height: 100%;
  cursor: pointer;
  position: absolute;
}
.text {
  font-size: 1.5rem;
  font-weight: 400;
}
.dragover {
  box-shadow: 0 0 20px;
  box-shadow: 0 0 0 0.2rem rgba(0,123,255,0.25);
  background-color: rgba(0,123,255,0.1);
}

.progress {
  width: 500px;
  height: 16px;
  background-color: #c5c5c5;
  border-radius: 16px;
  padding: 2px;
  .bar {
    background-color: #f5f5f5;
    width: 0;
    height: 100%;
    border-radius: 16px;
  }
}

</style>
