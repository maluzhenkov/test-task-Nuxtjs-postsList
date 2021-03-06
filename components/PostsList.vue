<template>
  <div class="post">
    <div v-if="!loading">
      <div class="alert" v-if="errors.length">Error: {{errors}}</div>

      <transition-group v-else-if="posts.length" tag="ul" class="post__list" name="fade" appear>
        <li class="post__item" v-for="post in posts" :key="post.id">
          <div class="post__header">
            <h3 class="post__title">{{ post.title }}</h3>
            <div class="spacer"></div>
            <button
              class="btn btn_close"
              type="button"
              v-if="!delItemLoading"
              @click.stop="delPost(post.id)"
            >&times;</button>
            <button class="btn btn_close" type="button" v-else disabled>
              <div class="spiner"></div>
            </button>
          </div>
          <div class="post__body">
            <div class="post__descr">{{ post.body }}</div>
          </div>
        </li>
      </transition-group>

      <div v-else>Posts not found</div>
    </div>

    <div class="loading" v-else>
      Posts Loading...
      <div class="spiner"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      delItemLoading: false
    };
  },
  computed: {
    posts() {
      return this.$store.getters.getPosts;
    },
    loading() {
      return this.$store.getters.getLoading;
    },
    errors() {
      return this.$store.getters.getErrors;
    }
  },
  methods: {
    delPost(id) {
      this.delItemLoading = true;
      this.$store
        .dispatch("delPost", id)
        .catch(err => {
          alert(err.message);
        })
        .finally(() => {
          this.delItemLoading = false;
        });
    }
  }
};
</script>

<style scoped>
.post {
  padding: 10px;
}
.post__header {
  display: flex;
  flex-wrap: nowrap;
  margin-bottom: 0.5rem;
}
.post__list {
  list-style-type: none;
  padding: 0;
}
.post__item {
  padding: 1rem 1rem 1rem 1.5rem;
  margin-bottom: 1rem;
  transition: all 0.3s ease-in-out;
  box-shadow: 0px 2px 5px 2px rgba(34, 60, 80, 0.1);
}

.post__title {
  overflow: hidden;
  text-transform: uppercase;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.loading {
  text-align: center;
}
.spiner {
  display: inline-block;
  width: 1rem;
  height: 1rem;
  border: 3px solid rgba(255, 255, 255, 0.5);
  border-radius: 50%;
  border-top-color: #000;
  animation: spin 1s ease-in-out infinite;
}

@keyframes spin {
  to {
    -webkit-transform: rotate(360deg);
  }
}
@-webkit-keyframes spin {
  to {
    -webkit-transform: rotate(360deg);
  }
}

.alert {
  padding: 1rem;
  background-color: #ff5722;
  color: white;
  border-radius: 5px;
}
.spacer {
  flex: 1;
  min-width: 50px;
}
.btn {
  border: none;
  background: transparent;
  color: #000;
  cursor: pointer;
}

.btn:focus {
  outline: none;
  background-color: rgb(192, 192, 192);
}
.btn_close {
  line-height: 1;
  font-size: 1.2em;
  font-weight: bold;
}

.fade-enter,
.fade-leave-to {
  opacity: 0 !important;
  transform: translateX(-15px) !important;
}

@media (min-width: 1024px) {
  .btn_close {
    transition: opacity 0.3s ease-in-out;
    opacity: 0;
  }
  .btn:hover {
    background-color: rgb(224, 224, 224);
  }
  .post__item:hover {
    box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
    transform: translateY(-1px);
  }
  .post__item:hover .btn_close {
    opacity: 1;
  }
}
</style>

