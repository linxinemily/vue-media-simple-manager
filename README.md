# Vue Media Simple Manager

> simple media mananger build with Vue.js 2.0.

## Intro

you can use these Vue components provided by *Vue Media Simple Manager* to handle images uploading and manage these images, including editting the `alt` and `title` of the images.


## Installation

```
$ npm i vue-media-simple-manager
```

## How to use

Register the components:
```
import { MediaGallery, MediaUploader } from 'vue-media-simple-manager'
export default {
    components: {
        MediaGallery, MediaUploader
    }
}
```

### Components Usage

the components require these props and defined methods:

#### MediaUploader

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

#### MediaGallery

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

### Data Format of Media

Each of your media should match the following format:

```

<table>
    <tr>
        <td>Property</td>
        <td>Type</td>
        <td>Description</td>
    </tr>
    <tr>
        <td>title</td>
        <td>String</td>
        <td>text of your media title(when data uploaded, the default value will be "title") </td>
    </tr>
    <tr>
        <td>url</td>
        <td>String</td>
        <td>the absolute URL of your media</td>
    </tr>
    <tr>
        <td>alt</td>
        <td>String</td>
        <td>alt of your media title(when data uploaded, the default value will be "alt") </td>
    </tr>
</table>
```

So, each of media Object would be like this:
```
{
    id: 1,
    title: "my stunning image"
    url: "https://example.com/api/image_01.jpg",
    alt: "image_01"
}

```


## Example of Usage

``` html
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

Full example is [here](https://github.com/linxinemily/vue-media-simple-manager/blob/master/src/index.vue)
