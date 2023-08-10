
<template>
    <div>
      <h1>Añadir nuevo video</h1>
      <div class="input-group mb-3">
        <input v-model="nuevaUrl"
          type="text"
          class="form-control"
          placeholder="Insertar url"
          aria-label="Insertar url"
          aria-describedby="basic-addon2"
        />
        <div class="input-group-append">
          <button class="btn btn-primary" type="button" @click="addVideo">Añadir</button>
        </div>
      </div>
    </div>
</template>
  
  <script lang="ts">
  import axios from 'axios';
  export default {
    props: ["getItems"],
    data() {
    return {
      nuevaUrl: '',
      apiKey : 'AIzaSyAYOE6GU8BgqV9r0NxU4ZveIr1STEUnijo',
      responseData: null,
      error: null,
      apiAws: 'https://e5if1zx2z8.execute-api.us-east-1.amazonaws.com/test/items'
    };
  },
    methods: {
      addVideo() {
        const url = this.nuevaUrl;
        const id = this.getId(url);
        if(id != null ){
        axios.get(`https://www.googleapis.com/youtube/v3/videos?id=${id}&key=${this.apiKey}&part=snippet`)
      .then(async response => {
        console.log(response.data)
        this.responseData = response.data.items[0].snippet;
        const data = {
        id: id,
        videoUrl: url,
        videoTitle : this.responseData.title,
        imageThumbnail: this.responseData.thumbnails.maxres || this.responseData.thumbnails.medium,
        videoDescription: this.responseData.description
      };

      try {
        await axios.put(this.apiAws, data);
        this.nuevaUrl = "";
        this.getItems();
      } catch (error) {
        console.error('Error:', error.message);
      }
      })
      .catch(error => {
        this.error = error.message;
        console.log(this.error)
        alert("Ha ocurrido un error!");
      });
        }

      },
      getId(url:string){
        const youtubeRegEx = /(?:\?v=|\/embed\/|\.be\/)([-a-zA-Z0-9_]{11})/;
        const match = url.match(youtubeRegEx);
            if (match && match[1]) {
              console.log(match[1])
                return match[1];
            } else {
                alert("URL de video de YouTube no válida");
                return null;
            }
      },
    },
  };
  </script>
  