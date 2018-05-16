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