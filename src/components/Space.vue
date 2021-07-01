
<template>
  <div id="space">

<transition name="slide-up">
    <div id="project-display" v-if="selectedProject">

        
    

    <div class="left">

        <div v-if="selectedProject.type == 'photobook'" class="photobook">
          
          <flipbook :nPolygons="5" class="flipbook" :zoom="[1]" :pages="getImagesForPhotobook(selectedProject.images)"></flipbook>

        </div>

         <div v-if="selectedProject.type == 'carousel'" class="photobook">



<div class="carousel">


    <agile id="carousel-slider" ref="carousel" :fade="true" :options="carouselOptions" :as-nav-for="navForOne">
        <div class="slide" v-for="(image, index) in selectedProject.images" :key="index">
            <img :src="image"/>
        </div>

        
    </agile>

  
  
<agile id="thumbnail-scroller" ref="thumbnails" autoplay :as-nav-for="navForTwo" :options="thumbnailsOptions" :slides-to-show="4">
  <div @click="$refs.carousel.goTo(index)" class="thumbnail-item" v-for="(image, index) in selectedProject.images" :key="index">
            <img :src="image"/>
        </div>
</agile>

           </div>
         </div>

    </div>

    <div class="right">
      <div class="information">
        <span class="project-title">{{selectedProject.title}}</span>
        <p class="project-description" v-html="selectedProject.description"></p>
      </div>
    </div>

    <span @click="selectedProject = null" class="close">X</span>

        
<!--<model-viewer auto-rotate camera-orbit="10.73deg 75deg 40250m" min-camera-orbit="auto auto auto" max-camera-orbit="auto auto 40250m" style="width: 1000px; height: 1000px" :src="'https://rozengallery.co.uk/' + selectedProject.model_src" camera-controls>

</model-viewer>-->

    </div>
</transition>

 <transition @after-enter="selectedProject = projects[selectedIndex]; selectedIndex = null; resetLoading(this);" name="slide">
    <div v-if="loading" class="loading">
         <!--<span class="loading-logo">ER</span>-->
         <img class="loading-image" src="@/assets/one.jpeg"/>
         
    </div>
  </transition>

  <span class="title">ELIZA ROZENTALE</span>
  <span class="subtitle">I N T E R A C T I V E   P O R T F O L I O</span>
  <span class="footer">MADE WITH &lt;3 BY <a href="https://cally.dev">CALLY.DEV</a></span>


    <div v-show="selectedProject == null" id="scene">

        <div @click="showProject(index)" class="project-card" @mouseover="hover = index" @mouseleave="hover = null" data-depth="0.1" :style="getCardStyle(project)" v-for="(project, index) in projects" :key="index">
          <transition name="fade">
            <span v-if="hover == index" :style="getCardTitleStyle(project)" class="card-title" v-html="project.name"></span>
          </transition>
          <img :src="require('@/assets/thumbnails/' + project.thumbnail)"/>
        </div>
       
        <!--<div  data-depth="0.2" class="card">
          <img @click="showProject('wtf')" src="@/assets/1.jpeg"/>
          
        </div>
<div data-depth="0.25" class="card-title">REDESIGN 2021</div>-->

      </div>
  </div>
</template>

<script>

import Parallax from 'parallax-js'
import Flipbook from 'flipbook-vue'
import { VueAgile } from 'vue-agile'
//import { ModelViewer } from '@google/model-viewer';

export default {
  name: 'Space',
  props: {
  },
  components: {
    //ModelViewer
    Flipbook,
    agile: VueAgile
  },
  mounted() {

    var scene = document.getElementById('scene');
    // eslint-disable-next-line no-unused-vars
    var parallax = new Parallax(scene);
    parallax.scalarX = 300;
    parallax.scalarY = 300;
    parallax.frictionX = 0.02;
    parallax.frictionY = 0.02;
    console.log(parallax);

    this.navForOne.push(this.$refs.thumbnails)
		this.navForTwo.push(this.$refs.carousel)

    },
  data () {
    return {
      page: {
        height: 100,
        width: 100
      },
      navForOne: [],
      navForTwo: [],

      thumbnailsOptions: {
        autoplay: true,
					autoplaySpeed: 5000,
					centerMode: true,
					dots: false,
          infinite: true,
					navButtons: false,
					slidesToShow: 3,
					responsive: [
						{
							breakpoint: 600,
							settings: {
								slidesToShow: 5
							}
						},
						{
							breakpoint: 1000,
							settings: {
								navButtons: false
							}
						}
					]
      },
      carouselOptions: {
            navButtons: false,
            infinite: true,
            responsive: [
                {
                    breakpoint: 600,
                    settings: {
                        dots: false
                    }
                },
                
                {
                    breakpoint: 900,
                    settings: {
                        dots: false,
                        infinite: false
                    }
                }
            ]
        },
      hover: null,
      selectedIndex: null,
      selectedProject: null,
      loading: false,
      projects: [
        

        {
          name: "SIGRADA<br>FAMILIA",
          thumbnail: "G3.jpg",
          height: "auto",
          width: "500px",
          x: "450px",
          y: "0px",
          type: "carousel",
          images: Array(6).fill(0).map((e,i)=>'https://rozengallery.co.uk/carousels/Sagrada/' + (i+1) + '.jpg'),
          title: "SAGRADA FAMILIA",
          description: "Drawn 2021<br><br>Sagrada is Anton Gaudis most famous Architectural masterpiece. The church, still laying under construction, was Gaudis lifelong work, which he spent his last years on.<br><br>Imaginary church underlies surreal, mythologic and metaphorical concepts, containing abstract iconography and episodic biblical references. It was designed Such that the building can be read by those seeking catholic faith, but at another level, studied theologically.<br><br>The true inspiration comes directly resembling nature, visually and technically. Gaudi believed that nature is a gift and nothing man-made should ever be higher than work of God. <br>Hence the height of the church drops few meters below the height of mountain Montjuic, Barcelona’s highest point. <br><br>Nature is displayed in almost every part of the church: the tree like columns, supporting the monument; fruits and herbs decorating the pillars; it can also be assumed that the positioning of the church embraces the colors of sunrise and sunset. <br><br>The church has three facades, each connected to a moment in life of Christ: the birth, passion/death, and his eternal glory. The positioning of the facades is believed as metaphor, a start of life and death. The façade of Christs births is Nativity façade and stands facing the sunrise, whilst the façade of Passion, and Christs death, where it sets.<br><br>There are literal remarks to biblical theme even within the structures of the church. Recently translated to build form, within the towers are Bishops symbols: mitre, staff and ring."
        },

        {
          name: "GAUDI",
          thumbnail: "gaudi.jpg",
          height: "auto",
          width: "400px",
          x: "-125px",
          y: "600px",
          type: "photobook",
          images: Array(18).fill(0).map((e,i)=>'https://rozengallery.co.uk/photobooks/gaudi/' + (i+1) + '.jpg'),
          title: "GUADI PROJECT",
          description: "Drawn 2020<br><br>The photobook explores the Spanish architects, Antoni Gudis, work. <br><br>When visiting Barcelona for the first time in 2019, Gaudis work left an impression on me. The details of the buildings where phenomenal, with vividly tiled blues, yellows and greens in the Casa Batllo and Park Guell, as well as the detailed stories told through the statues and details in La Sagrada Familia. The shapes and unsymmetric symmetry, that allowed me to interpret his work in a whole new way.<br><br>Gaudi, inspiring his work by nature, resembles it back into his work, with structures replicating trees in the woods, and shapes that are organic; the movement of nature was also considered, with La Sagrada Familias facades facing in directions of sun rising and setting, such that it enters the coloured glass windows, decorating the inside of the church in cold tones, during sunrise, and warm tones, during sunset. Whilst visiting the church during a sunrise, I got to experience the coloured shadows, enhancing the height of the building from the inside, and decorating it in red and orange tones."
        },

        {
          name: "WATERCOLOUR",
          thumbnail: "head.jpg",
          height: "auto",
          width: "400px",
          x: "1100px",
          y: "0px",
          type: "carousel",
          images: Array(4).fill(0).map((e,i)=>'https://rozengallery.co.uk/carousels/Watercolour/' + (i+1) + '.jpg'),
          title: "WATERCOLOUR",
          description: "Within the small series I experimented with building shapes of watercolour, to resemble the statues of Greek mythology. I aimed to understand the depths of shadows, and built the painting like pieces of puzzle, with no previous outline of the shape of the statue."
        },

        {
          name: "EMOTIONS",
          thumbnail: "6.png",
          height: "auto",
          width: "300px",
          x: "1150px",
          y: "700px",
          type: "photobook",
          images: Array(16).fill(0).map((e,i)=>'https://rozengallery.co.uk/photobooks/emotions/' + (i+1) + '.jpg'),
          title: "EMOTIONS",
          title_colour: 'brown',
          description: "Through colour, the photobook explores the journey of emotion of a young boy. From stages of curiosity and self-doubt, represented with orange and dark blue, to realisation and happiness, represented by uplifting yellow, the boy explores the unrealistic expectations the society has for him, and finds his identity past them."
        },

        {
          name: "PLATFORM 4",
          thumbnail: "13.png",
          height: "auto",
          width: "150px",
          x: "1650px",
          y: "1000px",
          type: "photobook",
          images: Array(22).fill(0).map((e,i)=>'https://rozengallery.co.uk/photobooks/platform4/' + (i+1) + '.png'),
          title: "PLATFORM 4",
          description: "Platform 4 captures the always changing, fast moving environment of a train station. <br><br>There is a certain way f mood during each part of the day, which the book captures in a story telling way; by noon, the tense, dark mornings will always be replaced by relaxed, smiling faces of some, or a rush and midday tiredness of others. <br><br>The pace at which the strangers replaced one another, moving through this build fascinated me. I realised how everyone had their own journey, we just all just crossed paths for a short time under the roof of the train station, on our way to Platform 4."
        },

        {
          name: "REU RESIDENCE",
          thumbnail: "building.png",
          height: "auto",
          width: "300px",
          x: "0px",
          y: "-300px",
          type: "carousel",
          title_colour: 'green',
          images: Array(9).fill(0).map((e,i)=>'https://rozengallery.co.uk/carousels/Sketchup/' + (i+1) + '.jpg'),
          title: "REU RESIDENCE",
          description: "The project is a Sketchup drawing, which is part of a Design Principles module group project. In this project, as a team we were to design a four-bedroom family house, for a pitch of land that would be provided to us. Besides following the British Building Standards, we were free to interpret our own design, and develop it within our groups.<br><br>As the pitch of land is located in our university campus, we considered the exposure of the building to students, that live and study on the campus. We decided to approach the project as a visual break for the students, as well as the new occupants of the building, from the surrounding urban London landscapes, bringing in the aspects of nature, and the nearby nature reservoir. <br><br>Within our design we included a wall, covered in wooden panels, replicating the trees of the reservoir, and moss peeking through, to bring life to the building, as well as beneficially purify the surrounding air. The pond, positioned right outside the sunken living space, creates a secluded effect to the first floor. The sun reflection of water ripples would enter the house during the midday, creating closer to the nature feel within the house.<br><br>Stretching around the house, the steps crossing through the pond, reaching the outdoor office space; this was a specific consideration due to many more people completing work from home. The outside office space will create a feel of a journey from home. "
        },

        {
          name: "TO MY YOUNGER<br>SELF",
          thumbnail: "5.png",
          height: "auto",
          width: "300px",
          x: "1600px",
          y: "400px",
          type: "photobook",
          images: Array(22).fill(0).map((e,i)=>'https://rozengallery.co.uk/photobooks/younger/' + (i+1) + '.png'),
          title: "TO MY YOUNGER SELF",
          title_colour: 'rgba(200, 100, 100, 1)',
          description: "A small but meaningful photo series that asks the question: “what advice would you give to your younger self?’’<br><br>A small question that moved strangers, and made them open up about some of their previous life regrets, and some heartfelt advice."
        }
]
    }
  },
  methods: {
    shown(el, done) {
      console.log('appeared');
      console.log(el.style.opacity);
      el.style.opacity = 1;
      console.log(done);
    },
    getPageStyle() {
      return {
        height: this.page.height + "vh",
        width: this.page.width + "vw",
        
        position: "absolute"
      }
    },
    getCarouselItemBackground(image) {
      return {
        backgroundImage: "require('" + image + "')",
        backgroundColor: 'red'
      }
    },
    getCardTitleStyle(project) {
      return {
        color: project.title_colour != undefined ? project.title_colour : 'black',
      }
    },
    getCardStyle(project) {
      return {
        height: project.height,
        width: project.width,
        //backgroundColor: this.generateRandomColour(),
        marginLeft: project.x,
        marginTop: project.y,
        position: "absolute",
        opacity: 1
      }
    },
    generateRandomColour() {
      return '#'+Math.floor(Math.random()*16777215).toString(16);
    },
    showProject(index) {
      this.loading = true;
      this.selectedIndex = index;
    },
    resetLoading(el) {
      this.loading = false;
      console.log('reset');
      console.log(el);
    },

    getImagesForPhotobook(images) {
      var arr = [null];
      images.forEach((image) => {
        arr.push(image);
      })
      return arr;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300&display=swap');

@import url('https://fonts.googleapis.com/css2?family=ABeeZee&family=Antonio:wght@500;700&display=swap');
#space {
  background-color: white;
  height: 100%;
  width: 100%;
  position: absolute;
  overflow: hidden;
}

.flipbook {
  margin-top: 22vh;
  width: 100%;
  height: 500px;
  z-index: 102;
  padding: 5%;
}

.carousel {
  margin-top: 12vh;
  width: 100%;
  height: 600px;
  z-index: 102;
  padding: 5%;
}



#carousel-slider {
  margin-bottom: 20px;
}

.slide {
  height: 600px;
}

.slide img {
height: 100%;
				object-fit:contain;
				object-position: center;
				width: 100%;
}

.thumbnail-item {
  align-items: center;
			box-sizing: border-box;
			color: #fff;
			display: flex;
			justify-content: center;
  cursor: pointer;
				height: 80px;
				padding: 0 5px;
				transition: opacity .3s;
}

.thumbnail-item img {
  height: 100%;
				object-fit: cover;
				object-position: center;
				width: 100%;
}

.flipbook /deep/ .bounding-box {
  box-shadow:         2px 2px 2px 2px rgba(0, 0, 0, 0.1);
}
.left {
  width: 50%;
  height: 100%;
  float: left;
}

.right {
  width: 50%;
  height: 100%;
  float: left;
}

.right .information {
  width: 100%;
  height: 600px;
  padding: 100px;
  padding-top: 25vh;
  overflow-y: scroll;
}

.right .project-description {
  margin-right: 200px;
  font-family: 'ABeeZee', sans-serif;
}

.right .project-title {
  font-family: 'Antonio', sans-serif;
  font-weight: 700;
  color: black;
  font-size: 60px;
  text-decoration: underline;
}

@media (max-width: 1200px) {
 .left {
   width: 100%;
   height: 500px;
   float: left;
  }

  .right {
    width: 100%;
    height: auto;
    float: left;
  }

#thumbnail-scroller {
  width: 90%;
  padding: 0px;
  margin: 0px;
  }


.right .information {
width: 100%;
  height: 600px;
  padding: 5vw;
  padding-top: 25vh;
  overflow-y:visible;
  }

  .right .project-description {
    margin-right: 8vw;
    }

    .carousel {
      height: auto;
}

#project-display {
  overflow-y: scroll;
  }

.slide {
  height: 400px;
  }
}


#scene {
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
  height: 100%;
}


#project-display {
  position: fixed;
  width: 100%;
  height: 100%;
  background-color: white;
  z-index: 100;
}

.cursor-pos {
  position: fixed;
  top: 10px;
  left: 10px;
}


.title {
  font-family: 'Antonio', sans-serif;
  color: black;
  font-size: 60px;
  width: 400px;
  left: 50%;
  margin-left: -200px;
  text-align: center;
  margin-top: 30px;
  position: fixed;
  top: 0px;
  z-index: 999;
}

.subtitle {
  font-family: 'Roboto', sans-serif;
  font-weight: 700;
  color: black;
  font-size: 10px;
  width: 400px;
  left: 50%;
  margin-left: -200px;
  text-align: center;
  margin-top: 105px;
  position: fixed;
  top: 0px;
  z-index: 999;
  letter-spacing: 4.4px;
}

.footer {
  font-family: 'Antonio', sans-serif;
  color: black;
  font-size: 15px;
  right: 5px;
  position: fixed;
  bottom: 5px;
  z-index: 999;
}

.close {
  font-family: 'Antonio', sans-serif;
  color: black;
  font-size: 40px;
  position: absolute;
  top: 30px;
  left: 30px;
  z-index: 102;
}

.loading-logo {
  font-family: 'Antonio', sans-serif;
  color: white;
  font-size: 300px;
  width: 100%;
  height: 100%;
  text-align: center;
  margin-top: 20%;
  position: fixed;
  color: white;
}

.loading-image {
  height: 80%;
  margin-top: 5%;
  margin-left: 15%;
  position: fixed !important;
}

.card-title {
  position: absolute;
  bottom: 0px;
  left: 0px;
  font-family: 'Antonio', sans-serif;
  font-size: 5rem;
  z-index: 2;
}

.loading {
  background-color: black;
  position: fixed;
  top: 0px;
  left: 0px;
  width: 0px;
  height: 100%;
  z-index: 101;
  overflow: hidden;
  clip: rect(0, auto, auto, 0);
  display: flex;
}

.slide-enter-active {
  animation: slide-in 1.5s;
}

.slide-leave-active {
  animation: slide-out 1.5s;
}

.slide-up-leave-active {
  animation: slide-up .5s;
}



.project-card {
  overflow:visible;
  pointer-events: all;
}

.project-card img {
  width: 100%;
  height: 100%;

}



.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

@keyframes slide-in {
  0% {
    
  }
  80% {
    width: 100%;
  }
  100% {
    width: 100%;
  }
  
}

@keyframes slide-out {
  0% {
    width: 100%;
  }
  100% {
    width: 0%;
  }
  
}

@keyframes slide-up {
  0% {
    top: 0px;
  }
  100% {
    top: -100%;
  }
  
}





</style>
