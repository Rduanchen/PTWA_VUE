<template>
  <div class="blog">
    <SearchSidebar></SearchSidebar>
    <div class="content">
      <div class="photo-gallery">
        <div class="photo-item" v-for="article in articles" :key="article.id">
          <div class="photo-placeholder" @click="openModal(article)">
            <span
              ><img :src="resolvePhotoPath(article.photo)" alt="article-title"
            /></span>
          </div>
          <div class="photo-title">[{{ article.title }}]</div>
        </div>
      </div>
      <ArticleModal
        @close="closeModal"
        v-if="showModal"
        :article="selectedArticle"
        :resolvePhotoPath="resolvePhotoPath"
      ></ArticleModal>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ArticleModal from "../components/Blog/ArticleModal.vue";
import SearchSidebar from "../components/Blog/SearchSidebar.vue";
import articlesOriginal from "../assets/Articles.json";
export default {
  name: "Blog",
  components: {
    SearchSidebar,
    ArticleModal,
  },
  methods: {
    openModal(article) {
      this.selectedArticle = article;
      this.showModal = true;
    },
    closeModal() {
      (this.showModal = false), (this.selectedArticle = null);
    },
    resolvePhotoPath(photo) {
      const url = new URL(`../assets/img/${photo}`, import.meta.url).href;
      return url;
    },
  },
  data() {
    return {
      showModal: false,
      articles: articlesOriginal.blog,
    };
  },
};
</script>

<style scoped>
.blog {
  display: flex;
  flex: 1;
  height: 100vh;
}

SearchSidebar {
  margin-left: 300px;
  flex: 1;
  padding: 20px;
  overflow-y: auto;
}

.content {
  flex: 1;
  background-color: white;
  padding: 20px;
}

.backblog {
  background-color: transparent;
  border: none;
  display: flex;
  align-items: center;
  cursor: pointer;
  margin-bottom: 20px;
}

.photo-gallery {
  flex: 1;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;
  margin-left: 2rem;
  justify-content: center;
}

.photo-item {
  text-align: center;
  align-items: center;
  display: flex;
  flex-direction: column;
}

.photo-placeholder img {
  border-radius: 10px;
  width: 200px;
  height: 200px;
  object-fit: cover;
}

.photo-title {
  margin-top: 0.5rem;
  font-size: 1rem;
  color: #333;
  font-weight: bold;
}
</style>
