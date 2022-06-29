<template>
  <div>
    <welcome-page-guest></welcome-page-guest>

    <section class="posts">
        <div class="container py-4">
            <div class="row row-cols-1 row-cols-sm-2 row-cols-lg-3 row-cols-xl-4 g-3">

                <div class="col" v-for="post in postResponse.data" :key="post.id">
                    <div class="card h-100">

                        <img :src="post.image" alt="">

                        <div class="card-body">
                            <h5>{{post.title}}</h5>

                            <p>{{post.content}}</p>
                        </div>

                    </div>
                </div>

            </div>

            <div class="pagination mt-5 d-flex justify-content-center">
                <nav aria-label="Page navigation">
                  <ul class="pagination">
                    <li :class="(postResponse.current_page == 1) ? 'disabled page-item' : 'page-item'">
                      <a class="page-link" href="#" aria-label="Previous" @click="getPosts(postResponse.current_page - 1)">
                        <span aria-hidden="true">&laquo;</span>
                        <span class="visually-hidden">Previous</span>
                      </a>
                    </li>

<!--                     <li class="page-item active"><a class="page-link" href="#">1</a></li>

                    <li class="page-item"><a class="page-link" href="#">2</a></li> -->

                    <li :class="(page_num == postResponse.current_page) ? 'page-item active' : 'page-item'" v-for="page_num in postResponse.last_page" :key="page_num">
                        <a class="page-link" href="#" @click="getPosts(page_num)">
                            {{page_num}}
                            </a>
                        </li>

                    <li :class="(postResponse.current_page == postResponse.last_page) ? 'disabled page-item' : 'page-item'">
                      <a class="page-link" href="#" aria-label="Next" @click="getPosts(postResponse.current_page + 1)">
                        <span aria-hidden="true">&raquo;</span>
                        <span class="visually-hidden">Next</span>
                      </a>
                    </li>
                  </ul>
                </nav>
            </div>
        </div>
    </section>

    <aside>
        <div class="categories"></div>
    </aside>
  </div>
</template>

<script>
import WelcomePageGuest from "../components/WelcomePageGuest.vue";

export default {
  name: "App",

  components: {
    WelcomePageGuest,
  },

  data() {
    return {
      posts: "",
      postResponse: ''
    };
  },

  methods: {
    //chiamata ajax per raccattare i posts
    //prende in input la pagina che vogliamo visualizzare
    getPosts(postPage) {
            axios
      .get("/api/posts", {
        //cerca i risultati della pagina selezionata
        params: {
            page: postPage
        }
      })
      .then((response) => {
        console.log(response.data);

        this.posts = response.data;

        this.postResponse = response.data;

        //console.log(this.posts);
      })
      .catch((e) => {
        console.error(e);
      });
    }
  },

  mounted() {
    this.getPosts(1);
  },
};
</script>