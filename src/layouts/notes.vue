<template>
  <div class="page">
    <router-link class="btn_back" to="/notelist">
      <i class="fas fa-long-arrow-alt-left"></i>回上層
    </router-link>
    <img class="pimg" :src="thisNote.img" alt />
    <div class="pbox">
      <div class="ptitle">{{thisNote.title}}</div>
      <div class="ptagicons">
        標籤：
        <span v-bind:key="icon.id" v-for="icon in thisIconTags" v-html="icon.Icon+icon.name"></span>
      </div>
      <div class="pcontent" v-if="content==''" v-html="thisNote.txt"></div>
      <markdown-it-vue-light v-for=" (item,index) in content" :key="index" class="md-body" :content="item" :options="options" />
      <div class="pills">
        <a target="_blank" :href="thisNote.relateLink" v-if="thisNote.relateLink!=''">#連結</a>
        <span>{{thisNote.cratetime}}</span>
      </div>
    </div>
    <div id="disqus_thread"></div>
  </div>
</template>

<script>
import notelist from "../modelData/note.js";
import MarkdownItVueLight from "markdown-it-vue/dist/markdown-it-vue-light.umd.min.js";
import "markdown-it-vue/dist/markdown-it-vue-light.css";

export default {
  data() {
    return {
      thisNote: {},
      content: "",
      thisName: "",
      thisIconTags: [],
      options: {
        markdownIt: {
          linkify: true
        },
        linkAttributes: {
          attrs: {
            target: "_blank",
            rel: "noopener"
          }
        }
      }
    };
  },
  components: {
    MarkdownItVueLight
  },
  created() {
    this.thisName = this.$route.params.name;
    notelist.rowdata.map(item => {
      if (item.enTitle == this.thisName) {
        this.thisNote = item;
        if(typeof item.txt == "object"){
          this.content = item.txt;
        }
      }
    });
    var tags = notelist.tags;
    this.thisNote.tags.map(icon => {
      for (var i = 0; i < tags.length; i++) {
        if (icon == tags[i].name) {
          this.thisIconTags.push(tags[i]);
        }
      }
    });
  }
};
</script>

<style scoped lang="scss">
.page {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
  padding-top: 25px;
  h1 {
    font-size: 2rem;
  }
}
.pimg {
  max-width: 60%;
}
.pbox {
  padding: 0.75rem;
  width: 90%;
  margin: auto;
}
.ptitle {
  font-size: 2rem;
  font-weight: 900;
  padding: 12px 0;
  letter-spacing: .1rem;
  text-shadow: 1px 1px 1px #aa0;
}
.pcontent {
  line-height: 1.3rem;
  font-size: 0.85rem;
}
.ptagicons {
  font-size: 1.2rem;
  padding: 1.2rem 0;
}
.btn_back {
  width: 120px;
  text-align: center;
  padding: 0.25rem 0.1rem;
  color: #444;
  position: absolute;
  left: 0;
  top: -10px;
}
.pills {
  padding: 0.5rem 0;
}
.pills a,
.pills span {
  display: inline-block;
  font-size: 0.875rem;
  padding: 0.25rem 0.75rem;
  margin: 0 0.5rem 0 0;
  font-weight: 600;
  border-radius: 25px;
  background-color: #edf2f7;
}

.md-body{
  font-size: 1.2rem;
}

.markdown-body pre{
  background-color: rgb(94, 94, 172);
}

@media only screen and (max-width: 768px) {
  .btn_back {
    top: 0;
    left: -5%;
    font-size: 1.1rem;
  }
  .pimg {
    max-width: 90%;
    padding: 1rem 0;
  }
  .pbox {
    padding: 0.5rem;
    width: 95%;
    font-size: 1.4rem;
  }
  .ptitle {
    font-size: 1.4rem;
  }

  .ptagicons,
  .pcontent {
    font-size: 1.1rem;
    line-height: 1.7rem;
  }
}
</style>