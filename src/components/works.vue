<template>
    <v-window active-class = "windows" v-model = "place" :reverse = "reverse" :touch = "{left, right}">
      <v-window-item v-for = "(item, i) in database" :key = "i">
        <v-img eager :src = "item.disp_image_url" min-height = "calc(var(--vh, 1vh) * 100)"></v-img>
        <div class = "more-info" @click = "showInfo">
            <h2>^</h2>
            <h1>{{item.title.toUpperCase()}}</h1>
            <h3>IN {{item.color.toUpperCase()}} {{item.material.toUpperCase()}}</h3>
            <div class = "break"></div>
            <template v-for = "(detail, i) in item.details">
              <v-img :src = "detail.image_url" :key = "i">
              </v-img>
              <h4>{{detail.caption}}</h4>
              <h4>{{detail.description}}</h4>
            </template>
            <div class = "break"></div>
            <div class = "break"></div>
            <h2>Message me if you're interested</h2>
            <div class = "break"></div>
        </div>
      </v-window-item>

    </v-window>
</template>

<script>
import mocbase from '../assets/mocbase.json'
  export default {
    name: 'HelloWorld',

    data: () => ({
      database: mocbase.database,
      place: 1,
      reverse: false,
      height: 0,
      show:false,
    }),

    methods: {
      left(){
        this.reverse = false;
        this.place++;
        this.show = false;
        document.documentElement.style.setProperty('--show', `${window.innerHeight}px`);
      },
      right(){
        this.show = false;
        document.documentElement.style.setProperty('--show', `${window.innerHeight}px`);
        this.reverse = this.place == 0
        this.place = (this.place - 1 + this.database.length)%this.database.length
      },
      showInfo(){
        if(this.show){
          this.show = false;
          document.documentElement.style.setProperty('--show', `${window.innerHeight}px`);
        }else{
          this.show = true;
          document.documentElement.style.setProperty('--show', `0px`);
        }
      }
    },

    computed:{
      screenHeigth(){
        return this.height + 'px'
      }
    },
    created(){
      let scrollY = 0;
      let moving = false;
      let vh = window.innerHeight * 0.01;
      document.documentElement.style.setProperty('--vh', `${vh}px`);
      document.documentElement.style.setProperty('--show', `${window.innerHeight}px`);

      window.addEventListener('resize', () => {
        // We execute the same script as before
        let vh = window.innerHeight * 0.01;
        document.documentElement.style.setProperty('--vh', `${vh}px`);
      });
      window.addEventListener('scroll', () => {
        scrollY = window.scrollY;
        if(!moving){
          if(scrollY > 1){
            moving = true;
            this.show = true;
            document.documentElement.style.setProperty('--show', `0px`);
            setTimeout(() => {moving = false}, 300)
          }else if(scrollY < -1){
            moving = true
            this.show = false;
            document.documentElement.style.setProperty('--show', `${window.innerHeight}px`);
            setTimeout(() => {moving = false}, 300)
          }
        }
      });
    }

  }
</script>

<style>
img{
  height: 100%;
}
.windows{
  height: 100%;
}
.more-info{
  position: fixed;
  top: calc(var(--show) - 30px);
  transition: 0.3s ease-in;
  width: 100%;
  height: calc(var(--vh, 1vh) * 100 + 30px);
  overflow: scroll;
  webkit-backdrop-filter: blur(calc(25px - var(--show)));
  backdrop-filter: blur(calc(25px - var(--show)));
  padding: 0 20pt;
  color: black;
  background: rgba(252, 250, 255, 0.3);

}
.scroll{
  overflow: hidden;
}
.more-info h2{
  text-align: center;
  line-height: 30px;
  padding-bottom: 20pt;
}
.break{
  height: 40px;
}
</style>
