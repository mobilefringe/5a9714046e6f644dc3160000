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
            <div class="post_container" v-if="posts" v-for="post in posts">
                <div class="post_image">
                    <img :src="post.image_url" :alt="post.title">
                </div>
                <div class="post_content">
                    <h2 class="post_heading">{{ post.title }}</h2>
                    <div class="post_text" v-html="post.body_short"></div>
                    <router-link :to="{ name: 'postDetails', params: { id: post.slug }}" class="post_read_more"  :aria="post.title">
					   {{ $t("blog_page.read_post") }} <i class="fa fa-angle-right" aria-hidden="true"></i>
				    </router-link>
                </div>
            </div>
            <button class="" v-if="!noMorePosts" @click="handleButton">Load More</button>
            <p v-if="noPosts">No More Posts</p>
        </div>
    </div>
</template>
<script>
    define(["Vue", "vuex", "jquery", "moment", "moment-timezone", "vue-moment", "vue-meta", "vue-social-sharing"], function (Vue, Vuex, $, moment, tz, VueMoment, Meta, SocialSharing) {
        return Vue.component("news-details-component", {
            template: template, // the variable template will be injected,
            props: ['id'],
            data: function () {
                return {
                    dataLoaded: false,
                    currentBlog: null,
                    mainBlog: null,
                    currentPost: null,
                    newsletterError: false,
                    newsletterSuccess: false,
                    socialFeed: null,
                    instaFeed: null
                }
            },
            created() {
                this.loadData().then(response => {
                    this.updateCurrentBlog(this.id);
                    this.dataLoaded = true;
                });
            },
            watch: {
                $route: function () {
                    this.updateCurrentBlog(this.$route.params.id);
                }
            },
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'timezone',
                    'blogs',
                    'findBlogByName',
                    'findBlogPostBySlug',
                ])
            },
            methods: {
                loadData: async function () {
                    try {
                        let results = await Promise.all([this.$store.dispatch("getData", "blogs")]);
                        return results;
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                },
                updateCurrentBlog(id) {
                    var blogName = "main";
                    this.currentPost = this.findBlogPostBySlug(blogName, id);
                    if (this.currentPost === null || this.currentPost === undefined) {
                        this.$router.replace({ name: '404' });
                    }
                },
                // tagString(val_tag) {
                //     // Returns all tags
                //     // var string = _.join(val_tag, ', ')  
                //     // return string
                    
                //     //Returns only the first tag
                //     var tag = "";
                //     if(val_tag != null){
                //         tag = val_tag[0].replace(/_/g, " ");
                //     }
                //     return tag
                // },
                // truncate(val_body) {
                //     var truncate = _.truncate(val_body, { 'length': 99, 'separator': ' ' });
                //     return truncate;
                // },
                shareURL(slug) {
                    var share_url = "http://www.northparkcenter.com/posts/" + slug
                    return share_url
                },
                
            }
        });
    });
</script>