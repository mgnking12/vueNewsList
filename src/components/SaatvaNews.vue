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
                          <a class="mail" v-bind:href="mailHref"></a>
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
      linkedinHref: '',
    }
  },
  methods: {
  
    sourceChanged: function(e) {
     for (var i=0; i<this.sources.length; i++) {
       if (this.sources[i].title == e.target.value) {
         this.source = this.sources[i];
         this.setupShare();
       }
     }

      var currentActive = document.getElementsByClassName("active");
      if(currentActive.length > 0){
        currentActive[0].classList.remove("active");
        currentActive = currentActive;
      }

     e.target.classList.add("active");
     this.$emit('sourceChanged', e.target.value);

     this.showImage(this.source.urlToImage, e);
    },
    showImage: function(image, event){
      var cards = document.querySelectorAll(".cards:not(.active)");
      for (var i=0; i<cards.length; i++) {
        cards[i].removeAttribute("style");
     }
      event.target.setAttribute("style", "background: linear-gradient(rgba(20,20,20, .5), rgba(20,20,20, .5)), url(" + image + ") no-repeat center; background-size: cover")
    },
    hideImage: function(){
      var cards = document.querySelectorAll(".cards:not(.active)");
      for (var i=0; i<cards.length; i++) {
        cards[i].removeAttribute("style");
     }
    },
    setupShare: function() { 
      this.mailHref = 'mailto:?subject=Check this out!&body=' + this.source.url;
        this.twitterHref = 'https://twitter.com/share?url=' + this.source.url;
        this.facebookHref = 'http://www.facebook.com/sharer.php?u=' + this.source.url;
        this.linkedinHref = 'https://www.linkedin.com/shareArticle?mini=true&url=' + this.source.url;
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