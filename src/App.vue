<template>
  <div>
    <div v-if = "!hide" :class = "{'load-screen':true, fade: loaded}">
      <div class = "center">
          <h1>GABRIEL</h1>
          <h1>LEONARD</h1>
          <h1>Φ</h1>
      </div>
    </div>
    <works v-if = "loaded" :database = "database"/>
  </div>
</template>

<script>
import works from './components/works';
import firebase from 'firebase'
// Your web app's Firebase configuration
 var firebaseConfig = {
   apiKey: "AIzaSyDniS0prRjEmOyKbMd4jequo9gkwe2otKI",
   authDomain: "fashion-galetora.firebaseapp.com",
   databaseURL: "https://fashion-galetora.firebaseio.com",
   projectId: "fashion-galetora",
   storageBucket: "fashion-galetora.appspot.com",
   messagingSenderId: "469438762797",
   appId: "1:469438762797:web:759f543ce82183b9f04da4",
   measurementId: "G-Q7DJ37H3D0"
 };
 // Initialize Firebase
 firebase.initializeApp(firebaseConfig);

export default {
  name: 'App',

  components: {
    works,
  },

  data: () => ({
    loaded: false,
    hide: false,
    database: {}
  }),
  created(){
    let vh = window.innerHeight * 0.01;
    document.documentElement.style.setProperty('--vh', `${vh}px`);
    document.documentElement.style.setProperty('--show', `${window.innerHeight}px`);

    window.addEventListener('resize', () => {
      // We execute the same script as before
      let vh = window.innerHeight * 0.01;
      document.documentElement.style.setProperty('--vh', `${vh}px`);
    });

    firebase.database().ref('test-database').once('value').then((sc) => {
      this.database = sc.val();
      this.loaded = true;
      setTimeout(() => {this.hide = true}, 500)
    })
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto+Mono:500&display=swap');
body{
  margin: 0;
  padding: 0;
  font-family: 'Roboto Mono', monospace;

}
.container{
  padding: 0;
  /* height: 100%; */
}
.load-screen{
  width: 100%;
  height: 100%;
  padding: 50px;
  position: fixed;
  z-index: 1000;
  background: rgba(255, 253, 245);
  transition: 0.5s ease-in;
}
.load-screen h1{
  text-align: center;
  line-height: 50px;
}
.load-screen:not(.fade){
  opacity: 1;
}
.fade{
  opacity: 0;
}
.center{
  position: fixed;
  height: 150px;
  margin: auto;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

</style>
