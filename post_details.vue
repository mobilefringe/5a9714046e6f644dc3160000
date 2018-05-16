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
    		<div class="row">
                <div class="col-md-12">
                    <router-link to="/posts">
                        <p class=""><i class="fa fa-chevron-left"></i> Back to BCC Blog</p>
                    </router-link>
                </div>
            </div>
            <div class="row" v-if="currentPost">
                <div class="col-md-12">
                    <div class="post_details_image">
                        <img :src="currentPost.image_url" :alt="currentPost.title" />
                    </div>
                    <div class="details_share_icons hidden_phone">
                        <a href="" target="_blank">
                            <img class="details_icon" src="//codecloud.cdn.speedyrails.net/sites/5808c7e76e6f6414b30c0000/image/jpeg/1490794350000/fb.jpg" alt="Facebook Logo">
                        </a>
                        <a href="" target="_blank">
                            <img class="details_icon" src="//codecloud.cdn.speedyrails.net/sites/5808c7e76e6f6414b30c0000/image/jpeg/1490794359000/tw.jpg" alt="Twitter Logo">
                        </a>
                    </div>
                </div>
                <div class="col-md-12">
                    <div class="blog_30 hidden_phone"></div>
                    <div class="row_fluid">
                        <h2 class="post_details_title">{{ currentPost.title }}</h2>
                        <div class="post_details_desc" v-html="currentPost.html_body"></div>
                    </div>
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