<template>
  <div class="carditem">
    <h2><span></span><span>{{cardtopic}}</span><span></span></h2>
    <div class="maincontent">
      <div class="first">
        <img :src="firstImg" alt />
        <h3>{{firstTitle}}</h3>
        <p v-if="firstTxt!=''" :class="{txtMore:isTxtMore}" v-html="firstTxt"></p>
        <!-- <div class="flex justify-end"><button class="inline-block bg-blue-500 hover:bg-blue-500 text-white py-1 px-2 rounded-full" @click="txtMore">{{strMore}}</button></div> -->
        <div class="pills">
          <a target="_blank" :href="firstLink" v-if="firstLink!=''">#連結</a>
          <span>{{firstCratetime}}</span>
        </div>
      </div>
      <div class="others">
        <ul>
          <li @click="swithItem(item.id)" v-bind:key="item.id" v-for="item in rowdata">
            {{item.title}}
            <p>{{item.cratetime}}</p>
            <span v-html="item.icon"></span>
          </li>
        </ul>
      </div>
      <div class="spansbox">
        <span v-bind:key="item.id" v-for="item in dataTags" v-html="item.Icon+item.name"></span>
      </div>
    </div>
  </div>
</template>

<script>
import workdata from "../../modelData/work.json";
import lifedata from "../../modelData/life.json";
export default {
  data() {
    var dataObj;
    var rowdata;
    switch (this.dataTarget) {
      case "work": {
        dataObj = workdata;
        rowdata = workdata.rowdata;
        break;
      }
      case "life": {
        dataObj = lifedata;
        rowdata = lifedata.rowdata;
        break;
      }
    }
    //icon處理

    for (let i = 0; i < rowdata.length; i++) {
      var tagsArr = rowdata[i].tags;
      var icons = "";
      for (let k = 0; k < tagsArr.length; k++) {
        for (let j = 0; j < dataObj.tags.length; j++) {
          if (tagsArr[k] == dataObj.tags[j].name) {
            icons += dataObj.tags[j].Icon;
          }
        }
      }
      rowdata[i].icon = icons;
    }
    return {
      cardtopic: dataObj.cardtopic,
      firstImg: rowdata[0].img,
      firstTitle: "《" + rowdata[0].title + "》",
      firstTxt: rowdata[0].txt,
      firstLink: rowdata[0].relateLink,
      firstCratetime: "#" + rowdata[0].cratetime,
      rowdata: rowdata,
      isTxtMore: false,
      strMore: "more",
      dataTags: dataObj.tags
    };
  },
  name: "carditem",
  props: {
    dataTarget: String
  },
  methods: {
    swithItem: function(id) {
      var rowdata;
      switch (this.dataTarget) {
        case "work": {
          rowdata = workdata.rowdata;
          break;
        }
        case "life": {
          rowdata = lifedata.rowdata;
          break;
        }
      }
      var index = parseInt(id) - 1;
      this.firstImg = rowdata[index].img;
      this.firstTitle = "《" + rowdata[index].title + "》";
      this.firstTxt = rowdata[index].txt;
      this.firstLink = rowdata[index].relateLink;
      this.firstCratetime = "#" + rowdata[index].cratetime;
    },
    txtMore: function() {
      this.isTxtMore = !this.isTxtMore;
      this.strMore = this.isTxtMore ? "less" : "more";
    }
  }
};
</script>

<style scoped>
.carditem {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 1.25rem;
  opacity: 1;
}
.carditem h2 {
  font-size: 1.3rem;
  border-left: 12px solid transparent;
  border-image: 12 repeating-linear-gradient(
      -45deg,
      red 0,
      red .1em,
      transparent 0,
      transparent .2em,
      #58a 0,
      #58a .3em,
      transparent 0,
      transparent .4em
    );
  padding: 12px .5rem;
}
.maincontent {
  display: flex;
  justify-content: center;
  width: 85%;
  margin: auto;
  border-radius: 0.5rem;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1),
    0 4px 6px -2px rgba(0, 0, 0, 0.05);
  text-align: left;
  position: relative;
  flex-direction: row-reverse;
  padding: .75rem 0;
}

.first {
  flex-basis: 50%;
  flex-grow: 2;
  padding: 0px 0.75rem;
}
.others {
  flex-basis: 35%;
  flex-grow: 1;
  position: relative;
  max-height: 350px;
  overflow-x: hidden;
  overflow-y: scroll;
}

.spansbox {
  position: absolute;
  bottom: 5px;
  right: 5px;
  display: flex;
}

.spansbox span {
  padding: 0.25rem;
  font-size: 0.9rem;
  white-space: nowrap;
}

.first img {
  max-width: 24rem;
  margin-right: auto;
  border-radius: 0.5rem;
}
.first h3 {
  font-weight: 700;
  font-size: 1.25rem;
  margin: 0.5rem 0;
}

.first p {
  /* max-height: 85px;
  overflow: hidden; */
  font-size: 1rem;
  font-weight: 300;
}

.pills {
  padding: 0.5rem;
}
.pills a,
.pills span {
  display: inline-block;
  font-size: 0.875rem;
  padding: 0.25rem 0.75rem;
  margin: 0 0.5rem;
  font-weight: 600;
  border-radius: 25px;
  background-color: #edf2f7;
}

.others li {
  border-left: 4px solid #fbd38d;
  font-weight: 500;
  margin-bottom: 0.5rem;
  padding: 0px 0.5rem;
  cursor: pointer;
  font-size: 1rem;
}

/* .txtMore{
  max-height: 550px !important;
} */

button:focus {
  outline: transparent;
}

@media only screen and (max-width: 767px) {
  .carditem {
    width: 100%;
    overflow: hidden;
    padding: 0.5rem;
  }
  .maincontent {
    width: 98%;
    margin: auto;
    flex-direction: column-reverse;
    padding-bottom: 40px;
  }
  .spansbox {
    bottom: 5px;
  }

  .first img {
    max-width: 100%;
    border-radius: 15px;
    padding: 10px 0;
  }

  .carditem h2{
    border:none;
    border-image: none;
    text-align: center;
    width: 100%;
    position: relative;
  }
  .carditem h2 {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .carditem h2 span:first-child,.carditem h2 span:last-child{
    display: block;
    width: 25%;
    height: 0;
    border:1px solid #444;
    margin: 0px 4px;
    /* position: absolute;
    top: 50%;
    left: 0%; */
  }
}
</style>
