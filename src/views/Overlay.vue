<template>
  <div id="nav" :class="{ mobile: mobile }">
    <Loading :show="showLoading"></Loading>

    <Intro :show="!playedIntro"></Intro>


    <template v-if="route.name == 'Landscape' && playedIntro">
      <div id="nav-home" class="navbutton"  ><router-link @click.native="goHome" :to="{ name: 'Landscape', params: { slug: slug} }">Home</router-link></div>
        <div id="nav-intro" class="navbutton" @click="goToIntro" >Intro</div>
        <div id="nav-index" class="navbutton" ><router-link :to="{ name: 'Index', params: { slug: slug } }">Index<img class="forward" src="@/assets/interface/forward.svg"/></router-link></div>

        <transition name="fade">
          <Transcript />
        </transition>
    </template>

    <template v-if="!playedIntro">
      <div id="skipintro" @click="skipIntro">
        Skip intro<img class="forward" src="@/assets/interface/forward.svg"/>
      </div>
    </template>

    <template v-if="route.name == 'Landscape' && playedIntro">

        <About :show="showAbout" @close="aboutToggle"></About>
        <div id="logos" @click="aboutToggle">
          <!-- About -->
          <img :src="logo_risd" />
          <img :src="logo_cca" />
          <img :src="logo_plus" />
        </div>

    </template>

    <template v-if="route.name == 'Index'">
      <div id="indexnav">
        <router-link :to="{ name: 'Landscape', params: {slug: slug} }">
          <div class="buttontext returnbutton"><img class="back" src="@/assets/interface/back.svg"/>Return</div>
        </router-link>
      </div>
    </template>

    <SoundPlayer />

  </div>
</template>

<script>
/*  eslint-disable */ 

import Transcript from "@/components/Transcript.vue";
import About from "@/components/About.vue";
import Loading from "@/components/Loading.vue";
import Intro from "@/components/Intro.vue";
import SoundPlayer from "@/components/SoundPlayer.vue";
import "@fontsource/space-mono/400.css"

import { isMobile } from 'mobile-device-detect';

export default {
  name: "Overlay",
  data() {
    return {
      logo_risd: require('@/assets/logos/risd.png'),
      logo_cca: require('@/assets/logos/cca.png'),
      logo_plus: require('@/assets/logos/plus.png'),

      showAbout: false,
      mobile: isMobile,
    };
  },
  components: {
    About,
    Loading,
    Intro,
    Transcript,
    SoundPlayer,
  },
  computed: {
    slug() {
      return this.$route.params.slug;
    },
    playingPathId() {
      return this.$store.getters.playingPathId;
    },
    route() {
      return this.$store.state.route;
    },
    playedIntro() {
      return this.$store.state.playedIntro;
    },
    showLoading() {
      return !this.$store.state.hasLoaded;
    },
  },
  methods: {
    aboutToggle: function() {
      this.showAbout = !(this.showAbout);
    },
    skipIntro: function() {
      this.$root.$emit('MapController_goHome')
      this.$store.commit("setPlayedIntro", true)
    },
    goHome() {
      this.$root.$emit('MapController_goHome')
    },
    goToIntro() {
      this.$root.$emit('MapController_goToIntro')
    },
  },
  mounted() {
  },

};
</script>
<style scoped lang="scss">


#nav {
  pointer-events: none;
  font-family: 'Space Mono', serif;
  position: fixed;
  z-index: 10000;
  top: 0px;
  right: 0px;
  left: 0px;
  bottom: 0px;
}

#nav > * {
  pointer-events: all;
}

.navbutton {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  position: fixed;
  z-index: 10000;
  font-size: 1.3em;

  background-color: #f5f5f5;
  padding: 16px 24px;  
  border-radius: 50px;
  box-shadow: -2px 6px 40px rgba(245, 245, 245, .8), 0px 0px 1.5px #212121;
  mix-blend-mode: hard-light;
  margin: 20px 0 0 20px;
  animation: floating 5s infinite;
  animation-timing-function: ease;
  transition: background-color 2s ease;
}

.mobile .navbutton {
  padding: 12px 12px;
  font-size: 0.8em;
  margin-left: 20px;
}


a {
  color: black;
  text-decoration: none;
}

#nav-home {
  left: 0px;
  top: 0px;
}

#nav-intro {
  left: 120px;
  top: 0px;
  cursor: pointer;
  transition: background-color 2s ease;

  .mobile & {
    left: 70px;
  }
}


#nav-index {
  right: 20px;
  top: 0px;
}


#indexnav {
  pointer-events: none;
  font-family: 'Space Mono', serif;
  position: fixed;
  z-index: 1000;
  top: 0px;
  right: 0px;
  left: 0px;
  bottom: 0px;
}

#indexnav > * {
  pointer-events: all;
}

.returnbutton {
  display: inline-block;
  position: relative;
  text-align: center;
  color: #212121;
  background: #f5f5f5;
  padding: 16px 24px;
  border-radius: 50px;
  box-shadow: -2px 6px 40px rgba(245, 245, 245, .8), 0px 0px 1.5px #212121;
  mix-blend-mode: hard-light;
  font-size: 1.3em;
  margin: 25px 0 0 20px;
  transition: background-color 2s ease;
  animation: floating 5s infinite;
  animation-timing-function: ease;
  vertical-align: middle;
}

.returnbutton:hover {
    background-color: rgba(235, 227, 220, .8);
}

.back {
  margin-right: 16px;
  display: inline;
  vertical-align: middle;
}

.forward {
  margin-left: 16px;
  display: inline;
  vertical-align: middle;
}

#nav-home:hover {
    background-color: rgba(235, 227, 220, .8);
}

#nav-index:hover {
    background-color: rgba(235, 227, 220, .8);
}

#nav-intro:hover {
    background-color: rgba(235, 227, 220, .8);
}

#skipintro:hover {
    background-color: rgba(235, 227, 220, .8);
}

#logos:hover {
    background-color: rgba(235, 227, 220, .8);
}


#logos {
  position: fixed;
  right: 20px;
  bottom: 15px;
  cursor: pointer;
  background: #f5f5f5;
  mix-blend-mode: hard-light;
  padding: 12px;
  border-radius: 60px;
  box-shadow: -2px 6px 40px rgba(245, 245, 245, .8), 0px 0px 1.5px #212121;
  transition: background-color 2s ease;
  animation: floating 5s infinite;
  animation-timing-function: ease;
  font-size: 1.3em;
  vertical-align: middle;

  .mobile & {
    padding: 8px; 
    display: flex;
    flex-direction: column;
  }
}

#logos img {
  max-height: 50px;
  max-width: 50px;
  margin: 0 .5em;

  .mobile & {
    max-height: 25px;
    max-width: 25px;
    margin-top: 10px;
  }
}

#skipintro {
  position: fixed;
  bottom: 20px;
  left: 20px;
  cursor: pointer;
  background-color: #f5f5f5;
  padding: 16px 24px;  border-radius: 50px;
  box-shadow: 0px 0px 40px rgba(110, 167, 252, .5), 0px 0px 1.5px #212121;
  mix-blend-mode: hard-light;
  margin: 25px 20px 0 0;
  animation: floating 5s infinite;
  animation-timing-function: ease;
  transition: background-color 2s ease;
  z-index: 10000;
}

@keyframes floating {
  0%   {transform: translatey(-8px);}
  50%  {transform: translatey(0px);}
  100% {transform: translatey(-8px);}
}

// .buttontext {
//   position: absolute;
//   top: 30px;
//   left: 30px;
//   color: black;
//   font-size: 1.3em;
//   z-index: 10001;
// }


.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}


</style>

