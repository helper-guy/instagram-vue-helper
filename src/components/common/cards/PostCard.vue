<template>
  <div>
    <div class="container post border border-secondary p-0">
      <div class="d-flex flex-column">
        <div class="user p-2" style="flex: 1">
          <div class="d-flex justify-content-between align-items-center">
            <div class="d-flex">
              <div class="img-fluid">
                <img :src="user.profile_img" alt="" class="user-profile-post border border-warning" />
              </div>
              <div class="d-flex flex-column">
                <span>{{ user.userName }}</span>
                <span class="fs-6">{{ user.name }}</span>
              </div>
            </div>
            <i class="bi bi-three-dots pe-2 fs-4"></i>
          </div>
        </div>
        <div class="post-img mt-0" style="flex: 10">
          <img :src="thisPost.img" alt="" />
        </div>
        <div class="post-interaction d-flex flex-column" style="flex: 3">
          <div class="d-flex justify-content-between align-items-center">
            <div class="d-flex">
              <i
                class="bi pe-2 ps-2 fs-4 icon"
                @mouseenter="toggleHover(true)"
                @mouseleave="toggleHover(false)"
                @click="toggleLike"
                :class="{
                  'bi-heart-fill': isLiked,
                  'text-danger': isLiked,
                  'bi-heart': !isLiked,
                }"
              ></i>
              <i
                class="bi bi-chat pe-2 ps-2 fs-4 icon"
                @mouseenter="toggleHover(true)"
                @mouseleave="toggleHover(false)"
              ></i>
              <i
                class="bi bi-send pe-2 ps-2 fs-4 icon"
                @mouseenter="toggleHover(true)"
                @mouseleave="toggleHover(false)"
              ></i>
            </div>
            <i class="bi bi-bookmark pe-2 fs-4"></i>
          </div>
          <div class="likes-count ps-2">
            <span>{{ likesSize }} likes</span>
          </div>
          <div class="likes-count ps-2">
            <span>{{ user.userName }} {{ thisPost.caption }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import User from "../../../classes/User";
import store from "../../../stores/store";
import _ from "lodash";
import Post from "../../../classes/Post";

export default {
  name: "PostCard",
  data() {
    return {
      user: {} as User,
      isHovered: false,
      isLiked: false, 
    };
  },
  props: {
    post: Post,
  },
  created() {
    console.log("postCard created",store.state) 
    this.user = store.getters.getUserById(this.post?.belongsTo);
    console.log(this.post) 
    console.log("postCard user",this.user) 
  },
  methods: {
    toggleHover(hovered: boolean) {
      this.isHovered = hovered;
    },
    toggleLike() {
      this.isLiked = !this.isLiked;
      if (this.isLiked) {
        store.commit("likedPost", this.post?.id);
      } else {
        store.commit("dislikedPost", this.post?.id);
      }
    },
  },
  computed: {
    thisPost() {
      return store.getters.getPostById(this.post?.id);
    },
    likesSize(){
      return _.size(this.thisPost.likes);
    }
  },
};
</script>

<style scoped>
.post {
  display: flex;
  width: 542px;
  height: 760px;
}

.user-profile-post {
  width: 35px;
  height: 35px;
  border-radius: 50%;
  margin-top: 11px;
  margin-right: 10px;
}
.fs-6 {
  font-size: 0.9rem !important;
}

.icon:hover {
  color: #808080; /* Change the color to your desired hover color */
}
</style>
