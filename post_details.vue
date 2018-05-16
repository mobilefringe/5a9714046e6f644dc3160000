<template>
    <div v-if="dataloaded">
        <div class="margin_60"></div>
		<div class="site_container inside_page_content page_content" v-if="currentPost">
    		<div class="row">
                <div class="col-md-12">
                    <router-link to="/posts">
                        <p class=""><i class="fa fa-chevron-left"></i> Back to Blog</p>
                    </router-link>
                </div>
            </div>
            <div class="row">
                <div class="col-md-12">
                    <div class="post_details_image">
                        <img :src="currentPost.image_url" :alt="currentPost.title" />
                    </div>
                </div>
            </div>
            <div class="row">
                <div class="col-md-8">
                    <div class="row_fluid">
                        <h2 class="post_heading caps">{{ currentPost.title }}</h2>
                        <p class="post_dates">{{ currentPost.publish_date | moment("MMM DD, YYYY", timezone) }}</p>
                        <div class="post_text" v-html="currentPost.html_body"></div>
                    </div>
                </div>
                <div class="col-md-4">
                    
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    define(["Vue", "vuex", "moment", "moment-timezone", "vue-moment", "vue-social-sharing"], function (Vue, Vuex, moment, tz, VueMoment, SocialSharing) {
        return Vue.component("post-details-component", {
            template: template, // the variable template will be injected,
            props: ['id'],
            data: function () {
                return {
                    dataloaded: false,
                    currentPost: null
                }
            },
            created() {
                this.loadData().then(response => {
                    this.updateCurrentBlog(this.id);
                    this.dataloaded = true;
                });
            },
            watch: {
                $route: function () {
                    this.updateCurrentBlog(this.$route.params.id);
                },
                currentPost: function () {
                    if(this.currentPost != null){
                        if (_.includes(this.currentPost.image_url, 'missing')) {
                            this.currentPost.image_url = "http://via.placeholder.com/1200x724";
                        }
                    }
                } 
            },
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'timezone',
                    'blogs',
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
                    var blogName = "Bramalea City Centre";
                    this.currentPost = this.findBlogPostBySlug(blogName, id);
                    if (this.currentPost === null || this.currentPost === undefined) {
                        this.$router.replace({ name: '404' });
                    }
                    console.log(this.currentPost)
                },
                shareURL(slug) {
                    var share_url = "http://www.northparkcenter.com/posts/" + slug
                    return share_url
                },
                
            }
        });
    });
</script>