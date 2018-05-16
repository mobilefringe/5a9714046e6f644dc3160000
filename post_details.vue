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
            <div class="col-md-6">
                <h5 class="pull-left"><a href="/posts"><img src="//codecloud.cdn.speedyrails.net/sites/5808c7e76e6f6414b30c0000/image/png/1490621978000/left.png" class="post_details_arrow" alt=""> Back to Our Style</a></h5>
            </div>
        </div>
            <div class="post_container" v-if="currentPost">
                
            </div>
        </div>
    </div>
</template>
<script>
    define(["Vue", "vuex", "jquery", "moment", "moment-timezone", "vue-moment", "vue-meta", "vue-social-sharing"], function (Vue, Vuex, $, moment, tz, VueMoment, Meta, SocialSharing) {
        return Vue.component("post-details-component", {
            template: template, // the variable template will be injected,
            props: ['id'],
            data: function () {
                return {
                    dataloaded: false,
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
                    var blogName = "Bramalea City Centre";
                    this.currentPost = this.findBlogPostBySlug(blogName, id);
                    if (this.currentPost === null || this.currentPost === undefined) {
                        this.$router.replace({ name: '404' });
                    }
                    console.log(this.currentPost)
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