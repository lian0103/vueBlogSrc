<template>
  <div class="vinfo">
    <div class="logo">
      <img :src="catImg" alt="" class="h-16 w-16 rounded-full p-1">
      <p class="align-middle p-1 lh80 fontIkea">{{title}}</p>
    </div>
    <div class="infoMenu">
      <p class="p-1"><i class="far fa-compass"></i>by Jason Lien</p>
    </div>
    <hr/>
  </div>
</template>

<script>
import catImg from "../assets/img/cat.jpeg";
export default {
  data(){
    return{
      title:"MyBlog",
      catImg:catImg
    }
  },
  name: "vinfo",
  props: {
    msg: String
  }
};
</script>

<style scoped>
.vinfo {
  width: 100%;
  max-width: unset !important;
  height: 85px;
  flex-basis: 85px;
  flex-grow: 0;
  flex-shrink: 0;
  position: relative;
  /* box-shadow:  0 2px 4px 0 rgba(0,0,0,.06); */
  display: flex;
  justify-content: center;
  background-color: #fff;
}

.logo{
  display: flex;
  text-align: center;
  padding: .25rem;
}

.infoMenu{
  position: absolute;
  top: 25px;
  right: 20px;
  font-size: 1rem;
  color: #444;
  text-align: left;
}
.lh80{
  line-height: 60px;
}

@media only screen and (max-width: 767px){
  .vinfo {
    max-width: 100vw;
    flex-basis: 65px;
    justify-content: flex-start;
  }
  .logo{
    padding: .5rem;
  }
  .lh80{
    line-height: 60px;
  }
}

</style>
