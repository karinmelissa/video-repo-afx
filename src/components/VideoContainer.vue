<template>
    <ul class="no-bullets">
      <li v-for="(video, index) in videos" :key="video.id" style="margin-bottom: 10px;">
        <div class="card img-container" style="width: 18rem;">
            <img class="card-img-top" :src="video.imageThumbnail.url" alt="Card image" data-toggle="modal" :data-target="'#videoModal-' + index">
            <div class="card-body">
                <button type="button" class="btn btn-danger"  data-toggle="modal" :data-target="'#exampleModal-' + index">Eliminar</button>
                <div class="modal fade" :id="'exampleModal-' + index" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
                <div class="modal-dialog modal-dialog-centered" role="document">
                    <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">¿Seguro que quieres eliminar este video?</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Cerrar</button>
                        <button type="button" class="btn btn-primary" @click="removeVideo(video.id)"  data-dismiss="modal">Eliminar</button>
                    </div>
                    </div>
                </div>
                </div>
                <div class="modal fade" :id="'videoModal-' + index" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true" @hide="stopVideoInModal(index)">
                <div class="modal-dialog modal-dialog-centered modal-lg" role="document">
                    <div class="modal-content">
                      <div class="modal-header">
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                          <span aria-hidden="true">&times;</span>
                        </button>
                      </div>
                    <div class="modal-body d-flex">
                      <div class="embed-responsive embed-responsive-16by9 modal-item">
                        <iframe :id="'videoIframe-' + index" class="embed-responsive-item" :src="getVideourl(video.id)" allowfullscreen></iframe>
                      </div>
                      <div class="modal-item px-3">
                      <h3>{{video.videoTitle}}</h3>
                      <p v-if="video.videoDescription.length > 250">{{ video.videoDescription.slice(0, 250) + '...' }}</p>
                      <p v-else>{{ video.videoDescription }}</p>
                      </div>
                    </div>
                    </div>
                </div>
                </div>
            </div>
        </div>
      </li>
    </ul>
</template>

<script>
import axios from "axios";
export default{
    props: ["videos","getItems"],
    data(){
    return{
        apiAWs: 'https://e5if1zx2z8.execute-api.us-east-1.amazonaws.com/test/items',
    }
    },
    methods: {
    getVideourl(id) {
        const url = `https://www.youtube.com/embed/${id}`

      return url;
    },
    removeVideo(id){
      console.log(id);
         axios.delete(`${this.apiAWs}/${id}`)
        .then(response => {
          console.log("Elemento eliminado:", response.data);
          this.getItems();
        })
        .catch(error => {
          console.error("Error al eliminar el elemento:", error);
        });
    },
    stopVideoInModal(index) {
      console.log(index)
      const iframe = document.querySelector(`#videoIframe-${index}`);
      console.log(iframe)
      if (iframe) {
        const videoPlayer = iframe.contentWindow.document.querySelector('video');
        if (videoPlayer) {
          videoPlayer.pause();
        }
      }
    },
  },
}
</script>

<style>
.img-container {
  margin: 5px;
   display: flex;
  flex-direction: column;
  height: 100%;
}
.card-body {
  display: flex;
  flex-direction: column;
  height: 100%;
}

ul.no-bullets {
  list-style-type: none; 
  padding: 0; 
  margin: 0; 
  display: flex;
  flex-wrap: wrap;
}
.modal-item{
  flex: 1 1 0;
}
</style>
