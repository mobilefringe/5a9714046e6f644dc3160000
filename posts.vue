<template>
    <div v-if="dataloaded">
        <!-- v-if="pageBanner"  -->
        <div class="page_header" v-bind:style="{ backgroundImage: 'url(http://via.placeholder.com/1920x300)' }"> <!-- { backgroundImage: 'url(' + pageBanner.image_url + ')' } -->
			<div class="site_container">
				<div class="header_content">
					<h1>{{ $t("blog_page.title") }}</h1>
				</div>
			</div>
		</div>
		<div class="site_container inside_page_content page_content">
            <div class="post_container" v-for="(post, index) in posts">
                <div class="post_image">
                    <img :src="post.image_url" :alt="post.title">
                </div>
                <div class="post_content">
                    <h2 class="post_heading">{{ post.title }}</h2>
                    <div class="post_text" v-html="post.body_short"></div>
                    <router-link :to="'/posts/'+ post.slug" class="post_read_more"  :aria="post.title">
					   {{ $t("blog_page.read_post") }} <i class="fa fa-angle-right" aria-hidden="true"></i>
				    </router-link>
                </div>
            </div>
            <button class="" @click="handleButton">Load More</button>
            <!--<mugen-scroll :handler="fetchData" :should-handle="!loading">-->
            <!--    loading...-->
            <!--</mugen-scroll>-->
        </div>
    </div>
</template>
<script>
    define(["Vue", "vuex", "moment", "moment-timezone", "vue-moment", "vue-lazy-load", "vue-mugen-scroll"], function (Vue, Vuex, moment, tz, VueMoment, VueLazyload, MugenScroll) {
        Vue.use(VueLazyload);
        Vue.use(MugenScroll)
        console.log(MugenScroll)
        return Vue.component("news-component", {
            template: template, // the variable template will be injected
            data: function () {
                return {
                    dataloaded: false,
                    loading: false,
                    
                    posts: [],
                    morePosts: [],
                    morePostsFetched: false
                }
            },
            created() {
                this.loadData().then(response => {
                    this.blogs;
                    this.dataloaded = true;
                    // this.currentSelection = this.blogs
                });
            },
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'timezone',
                    'repos',
                    'findRepoByName',
                    'blogs',
                    'findBlogByName'
                ]),
                pageBanner() {
                    // return this.findRepoByName("News").images
                },
                blogs() {
                    var blog = this.findBlogByName("Bramalea City Centre").posts;
                    _.forEach(blog, function(value, key) {
                        if (_.includes(value.image_url, 'missing')) {
                            value.image_url = "http://via.placeholder.com/350x150";
                        }
                        value.body_short = _.truncate(value.body, { 'length': 99, 'separator': ' ' });
                    });
                    console.log(blog)
                    blog = _.reverse(_.sortBy(blog, function (o) { return o.publish_date }));
                    blog = _.slice(blog, [0], [3]);
                    return blog
                }
            },
            methods: {
                loadData: async function () {
                    try {
                        let results = await Promise.all([this.$store.dispatch("getData", "repos"), this.$store.dispatch("getData", "blogs")]);
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                },
                // fetchData() {
                //     this.loading = true
                //     // ... the code you wanna run to fetch data
                //     this.blog = _.slice(this.blog, [4] )
                //     this.loading = false
                // }
                handleButton: function () {
                    if(!this.morePostsFetched){
                        // axios.get('chat/messagesmore').then((response) => {
                            // this.moreMessages = response.data;
                            this.posts = this.blogs.splice(0, 3);
                            this.morePostsFetched = true;
                        // });
                    }
                    var nextPosts = this.blogs.splice(0, 3);
                    // Add 3 more posts to posts array
                    this.posts.push(nextMsgs);
                }
            }
        });
    });
</script>