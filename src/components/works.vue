<template>
    <v-window active-class = "windows" v-model = "place" :reverse = "reverse" :touch = "{left, right}">
      <tr :class = "{'search-bar': true, 'show-search': showSearch}">
        <td style = "width: 100%">
          <input type = "text" class = "search" pattern="[A-Z]" v-model= "filter" placeholder="SEARCH KEYWORDS"/>
        </td>
        <td style = "padding-right: 10px">
          <v-icon color = "#fff" v-if = "filter.length > 0" @click = "filter = ''; showSearch = false">close</v-icon>
        </td>
        <td style = "padding-right: 10px">
          <v-icon color = "#fff" @click = "showSearch = false">search</v-icon>
        </td>
      </tr>
      <h1 style = "padding: 50px 20px" v-if = "databaseFiltered.length == 0">THERE'S NOTHING HERE FOR YOU</h1>
      <v-window-item v-for = "(item, i) in databaseFiltered" :key = "i + 'd'">
        <v-img eager :src = "item.disp_image_url" min-height = "calc(var(--vh, 1vh) * 100)"></v-img>
        <div :class = "{'more-info':true, scroll:show}" @click = "showInfo">
          <div style = "text-align: center">

            <v-icon class = "expand-icon">expand_less</v-icon>
          </div>
            <h1>{{item.title.toUpperCase()}}</h1>
            <h3>IN {{item.color.toUpperCase()}} {{item.material.toUpperCase()}}</h3>
            <div class = "break"></div>
            <div v-for = "(detail, j) in item.details" :key = "j + 'a'" class = "detail-card">
              <v-img :src = "detail.image_url" :key = "j + 'b'">
              </v-img>
              <div class = "detail-caption">
                {{detail.caption}}
              </div>
              <div class = "detial-description" :key = "j + 'c'">{{detail.description}}</div>
            </div>
            <div class = "break"></div>
        </div>
      </v-window-item>

    </v-window>
</template>

<script>
  export default {
    name: 'HelloWorld',

    props: {
      database: {
        type: Array,
        required: true
      }
    },
    data: () => ({
      place: 1,
      reverse: false,
      height: 0,
      show:false,
      scroll: 0,
      filter: '',
      showSearch: false,
      moving: false
    }),

    methods: {

      left(){
        this.reverse = false;
        this.place++;
        this.show = false;
        document.documentElement.style.setProperty('--show', `${window.innerHeight}px`);
        this.moving = true
        setTimeout(() => {this.moving = false}, 500)
      },
      right(){
        this.show = false;
        document.documentElement.style.setProperty('--show', `${window.innerHeight}px`);
        this.reverse = this.place == 0
        this.place = (this.place - 1 + this.database.length)%this.database.length
        this.moving = true
        setTimeout(() => {this.moving = false}, 500)
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
      },
      databaseFiltered(){
        if(this.filter.length > 1){
          var database_filtered = []
          for(var i in this.database){
            if(this.database[i].keywords.toUpperCase().indexOf(this.filter.toUpperCase())!= -1){
              database_filtered.push(this.database[i]);
            }
          }
          return database_filtered
        }else{
          return this.database
        }
      }
    },
    created(){
      let scrollY = 0;

      window.addEventListener('scroll', () => {
        scrollY = window.scrollY;
        this.scroll = scrollY;
        if(!this.moving){
          if(scrollY > 20){
            if(this.showSearch){
              this.showSearch = false;
            }else{
              this.show = true;
              document.documentElement.style.setProperty('--show', `0px`);
            }
            this.moving = true;
            setTimeout(() => {this.moving = false}, 500)
          }else if(scrollY < -20){
            if(this.show){
              this.show = false;
              document.documentElement.style.setProperty('--show', `${window.innerHeight}px`);
            }else{
              this.showSearch = true;
            }
            this.moving = true
            setTimeout(() => {this.moving = false}, 500)
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
  webkit-backdrop-filter: blur(calc(25px - var(--show)));
  backdrop-filter: blur(calc(25px - var(--show)));
  padding: 0 20pt;
  color: black;
  background: rgba(252, 250, 255, 0.3);

}
.scroll{
  overflow: scroll;
}
.expand-icon{
  text-align: center;
  line-height: 30px;
  padding-bottom: 20pt;
}
.break{
  height: 40px;
}
.detail-caption{
  font-size: 16px;
  font-style: italic;
  text-align: center;
  padding-bottom: 10px;
}
.detail-description{
  font-size: 18px;
}
.detail-card{
  padding-bottom: 30px;
}
.search{
  font-size: 20px;
  outline: none;
  border: none;
  padding: 10px;
  color: white;
}
.show-search{
  top: 0px;
}
.search-bar:not(.show-search){
  top: -50px;
}
.search:focus{
  outline: none;
  border: none;
}
.search-bar{
  transition: 0.3s ease-in;
  position: fixed;
  z-index: 100;
  background: rgb(50, 50, 55);
}
</style>
