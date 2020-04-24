<template>
  <div id="content">
    <div slot="header" class="clearfix">
      <span>博客</span>
    </div>
    <div class="article_wrap" v-for="(item, index) in articleList" :key="index">
      <div class="article_title" @click="articleDetail(item._id)">
        {{ item.title }}
      </div>
      <div class="article_info">
        <span class="article_info_date">发表于：{{ item.date }}</span>
        <!-- <span class="article_info_label"
          >标签：
          <span v-if="item.labels.length === 0">未分类</span>
          <el-tag
            v-else
            class="tag_margin"
            size="small"
            type="info"
            effect="plain"
            v-for="(tag, index) in item.labels"
            :key="index"
            >{{ tag }}</el-tag
          >
        </span> -->
      </div>
      <div class="article_gist">{{ item.gist }}</div>
      <div @click="articleDetail(item._id)" class="article_button article_all">
        阅读全文 >
      </div>
      <el-divider></el-divider>
    </div>
  </div>
</template>

<script>
export default {
  name: "article",
  data() {
    return {
      articleList: []
    };
  },
  async asyncData(context) {
    const { data } = await context.$axios.get("articleList");
    console.log(data);
    console.log('123')
    return { articleList: data };
  }
  // mounted: function() {
  //   this.$http.get("/api/articleList").then(
  //     response => (this.articleList = response.body.reverse()),
  //     response => console.log(response)
  //   );
  // },
  // methods: {
  //   articleDetail: function(id) {
  //     // 这边不能多一个斜杠 '/articleDetail/'  因为router定义的路由是 '/articleDetail:id'
  //     // 我把router改成 '/articleDetail/:id' 让前后端的路由规则一致
  //     this.$router.push("/articleDetail/" + id);
  //   }
  // }
};
</script>

<style scoped>
.article_wrap {
  padding: 40px;
}

.article_title {
  display: inline-block;
  color: #222;
  font-size: 34px;
  font-weight: 600;
  border-bottom: 1px solid white;
  cursor: pointer;
}

.article_title:hover {
  border-bottom: 1px solid #222;
}

.article_info {
  color: #999;
  font-size: 14px;
  padding-top: 8px;
}

.tag_margin {
  margin: 3px;
}

.article_gist {
  text-align: left;
  padding-top: 40px;
  padding-bottom: 40px;
  font-size: 16px;
}

.article_button {
  display: inline-block;
  padding: 3px 12px;
  border: 2px solid #222;
  color: #222;
  font-size: 14px;
  cursor: pointer;
}

.article_all:hover {
  color: white;
  background: #000;
  font-weight: 600;
}
</style>
