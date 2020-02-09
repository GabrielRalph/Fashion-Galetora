<template>
  <works/>
</template>

<script>
import works from './components/works';

export default {
  name: 'App',

  components: {
    works,
  },

  data: () => ({
    //
  }),
};
</script>

<style>
body{
  margin: 0;
  padding: 0;
}
.container{
  padding: 0;
  /* height: 100%; */
}
.v-window__conatiner{
  /* height: 100%; */
}
.v-window{
  /* height: 100%; */
}
</style>
