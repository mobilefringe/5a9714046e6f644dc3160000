<template>
    <div v-if="dataloaded">
        <div v-if="pageBanner" class="page_header" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
			<!--http://via.placeholder.com/1920x300-->
			<div class="site_container">
				<div class="header_content">
					<h1 v-if="locale=='en-ca'">{{currentPage.title}}</h1>
					<h1 v-else>{{currentPage.title_2}}</h1>
					<h2 style="display:none;">Scroll to  view page details</h2>
					<h3 style="display:none;">View page details</h3>
				</div>
			</div>
		</div>
		<div class="site_container inside_page_content page_content">
            <div class="margin_side_20" >
                
            </div>
        </div>
    </div>
</template>
<script>
    define(["Vue", "vuex", "moment", "moment-timezone", "vue-moment", "vue-meta", "vue-paginate", "vue-lazy-load"], function (Vue, Vuex, moment, tz, VueMoment, Meta, VuePaginate, VueLazyload) {
        Vue.use(Meta);
        Vue.use(VuePaginate);
        Vue.use(VueLazyload);
        return Vue.component("news-component", {
            template: template, // the variable template will be injected
            data: function () {
                return {
                    dataloaded: false,
                    // currentSelection: null,
                    // paginate: ['currentSelection'],
                    // selected: "Select A Category",
                    // categoryOptions: [
                    //     {'label': 'All', 'value': 'blogs'},
                    //     {'label': 'Beauty', 'value': 'blogBeauty'},
                    //     {'label': 'Charitable Partners', 'value': 'blogCharity'},
                    //     {'label': 'Children', 'value': 'blogChildren'},
                    //     {'label': 'Fashion', 'value': 'blogFashion'},
                    //     {'label': 'Holiday', 'value': 'blogHoliday'},
                    //     {'label': 'Lifestyle', 'value': 'blogLifestyle'},
                    //     {'label': 'Luxury', 'value': 'blogLuxury'},
                    //     {'label': 'Men', 'value': 'blogMen'},
                    //     {'label': 'NorthPark50', 'value': 'blogNorthPark50'}
                    // ]
                }
            },
            created() {
                this.loadData().then(response => {
                    this.dataloaded = true;
                    // this.currentSelection = this.blogs
                });
            },
            // watch: {
            //     selected: function selected() {
            //         if (this.selected.value == "blogs") {
            //             this.currentSelection = this.blogs;
            //         } else if (this.selected.value == "blogBeauty") {
            //             this.currentSelection = this.blogBeauty;
            //         } else if (this.selected.value == "blogCharity") {
            //             this.currentSelection = this.blogCharity;
            //         } else if (this.selected.value == "blogChildren") {
            //             this.currentSelection = this.blogChildren;
            //         } else if (this.selected.value == "blogFashion") {
            //             this.currentSelection = this.blogFashion;
            //         } else if (this.selected.value == "blogHoliday") {
            //             this.currentSelection = this.blogHoliday;
            //         } else if (this.selected.value == "blogLifestyle") {
            //             this.currentSelection = this.blogLifestyle;
            //         } else if (this.selected.value == "blogLuxury") {
            //             this.currentSelection = this.blogLuxury;
            //         } else if (this.selected.value == "blogMen") {
            //             this.currentSelection = this.blogMen;
            //         } else if (this.selected.value == "blogNorthPark50") {
            //             this.currentSelection = this.blogNorthPark50;
            //         } else {
            //             this.currentSelection = this.blogs;
            //         }
            //     }
            // },
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
                    console.log(blog)
                    // blog = _.reverse(_.sortBy(blog, function (o) { return o.publish_date }));
                    // return blog
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
                // tagString(val_tag) {
                //     var string = _.join(val_tag, ' , ');
                //     return string
                // },
                // truncate(val_description) {
                //     var truncate = _.truncate(val_description, { 'length': 199, 'separator': ' ' });
                //     return truncate;
                // }
            }
        });
    });
</script>