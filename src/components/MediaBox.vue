<template>
   <div class="dialogue" @click.self="$emit('closeBox')">
      <div class="mediaBox" ref="mediaBox">
        <div class="mediaBox-head">
          <div class="navigator">
            <div class="navigator-title">編輯媒體</div>
            <a class="navigator-arrow" @click.prevent="$emit('goToPrev')">&#8249;</a>
            <a class="navigator-arrow" @click.prevent="$emit('goToNext')">&#8250;</a>
          </div>
          <a class="close" @click.prevent="$emit('closeBox')"></a>
        </div>
        <div class="mediaBox-body">
          <div class="mediaBox-body-imgContainer">
            <div class="img">
              <img class="img-fluid"  :src="childCurrentMedia.url" :alt="childCurrentMedia.alt">
            </div>
          </div>
          <div class="mediaBox-body-content">
            <div class="form-group">
              <label for="url">圖片網址</label>
              <input readonly="readonly" type="text" name="url" :value="childCurrentMedia.url">
            </div>
            <div class="form-group">
              <label for="title">圖片標題</label>
              <input type="text" name="title" v-model="childCurrentMedia.title">
            </div>
            <div class="form-group">
              <label for="alt">替代文字</label>
              <input type="text" name="alt" v-model="childCurrentMedia.alt">
            </div>
            <button
              href="#"
              class="btn btn-primary"
              :disabled="!hasChange"
              @click="$emit('updateMedia', childCurrentMedia)"
            >
              確認修改
            </button>
            <a href="#" @click="$emit('deleteMedia')">刪除圖片</a>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  props: {
    currentMedia: {
      type: Object,
      required: true,
    }
  },
  data() {
    return {
      hasChange: false,
      childCurrentMedia: {}
    }
  },
  created() {
    this.childCurrentMedia = Object.assign({}, this.currentMedia)
    this.$watch('childCurrentMedia', function() {
      this.hasChange = true
    }, {
      deep: true,
    })
  },
  watch: {
    currentMedia() {
      this.childCurrentMedia = Object.assign({}, this.currentMedia)
      this.$nextTick(function () {
        this.hasChange = false
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.navigator {
  &-title {
    font-size: 20px;
    font-weight: 500;
    letter-spacing: 1px;
    line-height: 30px;
  }
  display: flex;
  &-arrow {
    display: inline-block;
    padding: 2px 10px;
    cursor: pointer;
    border: 1px solid #eee;
    margin: 0 2px;
    border-radius: 4px;
    &:hover {
      background-color: #fafafa;
    }
  }
}
.dialogue {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.6);
}
.mediaBox {
  height: calc(100% - 80px);
  width: calc(100% - 80px);
  background-color: #fff;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  overflow: hidden;
  &-head {
    border: 1px solid #eee;
    padding: 16px 40px;
    color: #0e0e0e;
    letter-spacing: 1px;
    font-size: 1.2rem;
    display: flex;
    justify-content: space-between;
    .close {
      display: inline-block;
      position: relative;
      opacity: 0.3;
      width: 28px;
      &:hover {
        opacity: 1;
      }
      &:before, &:after {
        position: absolute;
        left: 15px;
        content: ' ';
        height: 33px;
        width: 2px;
        background-color: #333;
      }
      &:before {
        transform: rotate(45deg);
      }
      &:after {
        transform: rotate(-45deg);
      }
    }
  }
  &-body {
    display: flex;
    height: calc(100% - 62px);
    &-imgContainer {
      padding: 40px;
      width: 65%;
    }
    &-content {
      padding: 40px;
      border-left: 1px solid #eee;
      margin-left: 20px;
    }
  }
}


.img {
  height: 100%;
   &-fluid {
     max-width: 100%;
     max-height: 100%;
     height: auto;
   }
}
input {
    border: 1px solid #dfdfdf;
    padding: 6px 10px;
    border-radius: 4px;
    margin: 4px 0;
    &[readonly="readonly"] {
      background-color: #fafafa;
    }
}
.form {
  &-group {
    margin-bottom: 10px;
  }
}
</style>
