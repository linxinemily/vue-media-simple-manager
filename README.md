# Media Library

## How to use

Register the component:
```
import { MediaGallery, MediaUploader } from 'vue-media-simple-manager'
export default {
    components: {
        MediaGallery, MediaUploader
    }
}
```

the components require these props and defined methods:

### MediaUploader

* Props
<table>
    <tr>
        <td>Prop Name</td>
        <td>Type</td>
        <td>Description</td>
    </tr>
    <tr>
        <td>apiUrl</td>
        <td>String</td>
        <td>the service stores your images file & data</td>
    </tr>
</table>

* Methods
<table>
    <tr>
        <td>Method Name</td>
        <td>Description</td>
    </tr>
    <tr>
        <td>uploadMedia(data)</td>
        <td>the method will bring a param which is an array of response data from your api server</td>
    </tr>
</table>

### MediaGallery

* Props
<table>
    <tr>
        <td>Prop Name</td>
        <td>Type</td>
        <td>Description</td>
    </tr>
    <tr>
        <td>medias</td>
        <td>Array</td>
        <td>the data of your medias</td>
    </tr>
</table>

* Methods
<table>
    <tr>
        <td>Method Name</td>
        <td>Description</td>
    </tr>
    <tr>
        <td>deleteMutiMedias(data)</td>
        <td>
            to handle deleting multiple medias simultaneously
        </td>
    </tr>
    <tr>
        <td>deleteMedia(data)</td>
        <td>
            to handle deleting single media
        </td>
    </tr>
    <tr>
        <td>deleteMutiMedias(data)</td>
        <td>
            to handle updating the information of your medias 
        </td>
    </tr>
</table>


## Example of Usage

```
<template>
  <div>
    <media-uploader @uploadMedia="createMedia" :apiUrl="apiUrl"/>
    <media-gallery
      :medias="medias"
      @deleteMutiMedias="deleteMutiMedias"
      @deleteMedia="deleteMedia"
      @updateMedia="updateMedia"
    />
  </div>
</template>

```

The full example is here
