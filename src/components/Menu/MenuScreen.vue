<template lang="html">
<div>

<section class="menu">
  <div class="sky"></div>
  <div id="cloud-scape">
<img src="/src/assets/img/cloud.png" class="cloud" id="cloud-1">
<img src="/src/assets/img/cloud2.png" class="cloud" id="cloud-2">
<img src="/src/assets/img/cloud3.png" class="cloud" id="cloud-3">
  </div>
    <transition @enter="slideUp2" appear>  <div class="skyline2"></div></transition>
  <transition @enter="slideUp" appear>  <div class="skyline"></div></transition>


<transition @enter="fadeInPage">  <div  v-if="timeUp" class="overlay">
        <div class="container">
          <div class="row">
            <div class="col-sm-12">
              <state-bar :stateText="stateText" @change="changed"></state-bar>
            </div>

          </div>
          <div class="row">
          <div class="col-sm-3 ">
          <transition @beforeEnter="test" @enter="scaleYEnter" @afterEnter="test" @enterCancel="test"
          @beforeLeave="test" @leave="test" @afterLeave="test" @leaveCanceled="test" :css="false" appear>  <side-bar></side-bar></transition>
          </div>
            <div class="col-sm-9 ">
              <transition @leave="scaleYLeave" @enter="scaleYEnter":css="false" appear mode="out-in"><router-view @pageLoad="stateChange" :characters="characters"></router-view></transition>
            </div>
          </div>

      </div>
      <!-- Image modal for Skills -->
      <transition @enter="fadeEnter" @leave="fadeLeave">
        <skill-modal :skill="skill" :show="showSkillModal" v-if="showSkillModal" @toggleSkillModal="showSkillModal=false" ></skill-modal>
      </transition>
      <!-- Image modal for Mail -->
      <transition @enter="fadeEnter" @leave="fadeLeave">
        <mail-modal :show="showMailModal" v-if="showMailModal" @toggleMailModal="showMailModal=false" ></mail-modal>
      </transition>
      <!-- Image modal for Quests -->
      <transition @enter="fadeEnter" @leave="fadeLeave">
        <image-modal :show="showImageModal" v-if="showImageModal" @toggleImageModal="showImageModal=false" :image="image" ></image-modal>
      </transition>
    </div></transition>
</section>


  </div>
</template>

<script>
import {
    eventBus
} from '../../main';
import SkillModal from '../SkillModal.vue';
import MailModal from '../MailModal.vue';
import ImageModal from '../ImageModal.vue';
import {
    AnimationMixin
} from '../../mixins/Animations';
import SideBar from './SideBar.vue';
import StateBar from './StateBar.vue';
export default {
    data: function() {
        return {
            showModal: false,
            duration: .2,
            stateText: '',
            showSkillModal: false,
            showMailModal: false,
            showImageModal: false,
            skill: null,
            image: null,
            cloudCount: 0,
            birdCount: 0,
            timeUp: false,
            cloudAnimations: [],

        }
    },
    computed: {
        characters() {
            return this.$store.getters.characters;
        }
    },
    mixins: [AnimationMixin],
    components: {
        sideBar: SideBar,
        stateBar: StateBar,
        skillModal: SkillModal,
        mailModal: MailModal,
        imageModal: ImageModal
    },
    methods: {
        fadeInPage(el, done) {
            let tl = new TimelineMax;
            let delay = el.dataset.index * .05;
            tl.from(el, .5, {
                opacity: 0,
                onComplete: done
            })
        },
        slideUp(el, done) {
            let tl = new TimelineMax;
            tl.from(el, 1.5, {
                y: 80,
                ease: Power2.easeOut,
                delay: 0,
                onComplete: done
            })
        },
        slideUp2(el, done) {
            let tl = new TimelineMax;
            tl.from(el, 3, {
                y: 80,
                ease: Power2.easeOut,
                onComplete: done
            })
        },
        stateChange(e) {
            this.stateText = e.stateText;
        },
        animateCloud(cloudId, delayAnimation) {
            let cloud = document.getElementById(cloudId);
            let tl = new TimelineMax();
            tl.set(cloud, {
                y: this.random(0, 400)
            });
            tl.to(cloud, this.random(35, 60), {
                ease: Power0.easeNone,
                delay: delayAnimation,
                x: document.body.clientWidth + 1000,
                repeat: -1

            })

        },
        random(min, max) {
            return Math.floor(Math.random() * (max - min + 1)) + min;
        },
        changed() {}
    },
    created() {
        eventBus.$on('toggleSkillModal', (skill) => {
            this.showSkillModal = !this.showSkillModal;
            this.skill = skill;
            document.addEventListener("keydown", (e) => {
                if (this.showSkillModal && e.keyCode == 27) {
                    this.showSkillModal = false;
                }
            })
        });
        eventBus.$on('toggleMailModal', () => {
            this.showMailModal = !this.showMailModal;
            document.addEventListener("keydown", (e) => {
                if (this.showMailModal && e.keyCode == 27) {
                    this.showMailModal = false;
                }
            })
        });
        eventBus.$on('toggleImageModal', (image) => {
            this.showImageModal = !this.showImageModal;
            this.image = image;
            document.addEventListener("keydown", (e) => {
                if (this.showImageModal && e.keyCode == 27) {
                    this.showImageModal = false;
                }
            })
        });
    },
    mounted() {
        // background animations
        // Chrome 1+
        var isChrome = !!window.chrome && !!window.chrome.webstore;
        if(isChrome){
          this.animateCloud('cloud-1', .5);
          this.animateCloud('cloud-2', 5);
          this.animateCloud('cloud-3', 7);
        }

        setTimeout(() => {
            this.timeUp = true;
        }, 1000);

    }
}
</script>

<style>
section.menu {
    display: flex;
    background-color: #252525;
    height: 100vh;
    width: 100vw;
    position: absolute;
    background-size: cover;
    background-attachment: fixed;
    overflow-y: scroll;
    overflow-x: hidden;
    /*background-image: url('/src/assets/img/skyline.png')*/
}

.skyline {
    height: 100vh;
    width: 100vw;
    position: absolute;
    background-size: cover;
    background-repeat: no-repeat;
    background-attachment: fixed;
    background-image: url('/src/assets/img/skyline.png')
}

.skyline2 {
    height: 100vh;
    width: 100vw;
    position: absolute;
    background-size: cover;
    background-repeat: no-repeat;
    background-attachment: fixed;
    background-image: url('/src/assets/img/skyline2.png')
}

.sky {
    background-size: cover;
    background-attachment: fixed;
    position: absolute;
    height: 100vh;
    width: 100vw;
    background-image: url('/src/assets/img/sky.jpg')
}

.container {
    z-index: 2;
}

#cloud-scape,
#bird-scape {
    position: absolute;
    height: 100vh;
    width: 100vw;
    overflow: hidden;
}

.cloud {
    opacity: .6;
    position: absolute;
    left: -50%;
}

.bird {
    position: absolute;
    left: 0;
    top: 0;
    height: 50px;
}
/*@media (min-width:1800px) {
  .container{
    width:70%;
  }
}*/
.sub-menu {
    overflow: hidden;
    /*background-color: rgba(40, 100, 201, 0.71);*/
    background: rgba(38, 140, 209, 1);
    background: -moz-linear-gradient(top, rgba(38, 140, 209, .9) 0%, rgba(108, 103, 245, 1) 100%);
    background: -webkit-gradient(left top, left bottom, color-stop(0%, rgba(38, 140, 209, 1)), color-stop(100%, rgba(108, 103, 245, 1)));
    background: -webkit-linear-gradient(top, rgba(38, 140, 209, .9) 0%, rgba(108, 103, 245, 1) 100%);
    background: -o-linear-gradient(top, rgba(38, 140, 209, .9) 0%, rgba(108, 103, 245, 1) 100%);
    background: -ms-linear-gradient(top, rgba(38, 140, 209, .9) 0%, rgba(108, 103, 245, 1) 100%);
    background: linear-gradient(to bottom, rgba(38, 140, 209, .8) 0%, rgba(108, 103, 245, 1) 100%);
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#268cd1', endColorstr='#6c67f5', GradientType=0);
    padding: 10px 20px;
    border-radius: 5px;
    border: 3px solid #f4f4f4;
    margin-bottom: 20px;
}

.sub-menu.button {
    height: 100%;
    width: 100%;
    display: flex;
    color: #f4f4f4;

    padding: 20px;
}

.sub-menu p {
    list-style: none;
    color: #f4f4f4;
    font-family: 'VT323', monospace;
    font-size: 25px;
    /*padding: 5px;*/
    transition: .3s ease all;
}

p,
h2,
h1,
h3,
h4,
h5,
li,
a {
    color: #f4f4f4;
    font-family: 'VT323', monospace;
}

.overlay {
    height: 100%;
    width: 100%;
    top: 0;
    left: 0;
    position: fixed;
    z-index: 999;
    align-items: center;
    justify-content: center;
    overflow-y: scroll;
    background-color: rgba(30, 30, 30, .6);
    overflow-x: hidden;
}

@media (max-width:500px) {
    .sub-menu {
        padding: 5px;
    }
}
</style>
