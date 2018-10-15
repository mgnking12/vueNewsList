<template>
  <div class="saatvaNews">
  <header>
      <div>Saatva News</div>
    </header>
    <div>
      <div class="col-sm-4">
        <button class="cards" v-for="source in sources" v-bind:value="source.title" @mouseover="showImage(source.urlToImage, $event)" @mouseout="hideImage()" v-on:click="sourceChanged">{{ source.title }}</button>
        
        </div>
        <div v-if="source" class="col-sm-8 article noMobile">
          <div class="content">
            <h2>{{ source.title }}</h2>
            <img class="img-fluid" v-bind:src="source.urlToImage" />
            <div id="share">
                          <a class="mail" v-bind:href="mailHref" target="_blank"></a>
                          <a class="facebook" v-bind:href="facebookHref" target="_blank"></a>
                          <a class="twitter" v-bind:href="twitterHref" target="_blank"></a>
                          <a class="linkedin" v-bind:href="linkedinHref" target="_blank"></a>
                        </div>
              <div v-html="source.long_description"></div>
            </div>
        </div>
      
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'sourceselection',
  data () {
    return {
      sources: [],
      source: '',
      mailHref: '',
      twitterHref: '',
      facebookHref: '',
      linkedinHref: ''
    }
  },
  methods: {
  
    sourceChanged: function(e) {
     for (var i=0; i<this.sources.length; i++) {
       if (this.sources[i].title == e.target.value) {
         this.source = this.sources[i];
         
       }
     }
     console.log(e.target.classList)

      var currentActive = document.getElementsByClassName("active");
      if(currentActive.length > 0){
        currentActive[0].classList.remove("active");
        currentActive = currentActive;
      }

     e.target.classList.add("active");
     this.$emit('sourceChanged', e.target.value);
    },
    showImage: function(image, event){
      this.backgroundImg= image;
      event.target.setAttribute("style", "background: linear-gradient(rgba(20,20,20, .5), rgba(20,20,20, .5)), url(" + image + ") no-repeat center; background-size: cover")
    },
    hideImage: function(){
      this.backgroundImg= "";
      event.target.style.background = ''
    },
    setupShare: function() { 
      this.mailHref = 'mailto:?subject=Check this out!&body=' + this.currentPost.url;
        this.twitterHref = 'https://twitter.com/share?url=' + this.currentPost.url;
        this.facebookHref = 'http://www.facebook.com/sharer.php?u=' + this.currentPost.url;
        this.linkedinHref = 'https://www.linkedin.com/shareArticle?mini=true&url=' + this.currentPost.url;
    },
  },
  created: function () {
    axios.get('https://s3-us-west-2.amazonaws.com/saatva-hiring/news.json')
      .then(response => {
        this.sources = response.data.articles;
      });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  
        

</style>