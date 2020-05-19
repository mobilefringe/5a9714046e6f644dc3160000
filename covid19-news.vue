<template>
    <div v-if="currentPage">
        <div class="page_header" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
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
                <div class="row event_container"  v-if="accessibilityData"  v-for="promo in accessibilityData">
					<div class="col-sm-12 col-md-12 event_dets_container">
						<h4 class="event_name caps">{{promo.notice_title}}</h4>
						<div class="event_thick_line"></div>
						<p class="event_dates">{{promo.service_completed_date | moment("MMMM D, YYYY", timezone)}}</p>
						<p class="event_desc" v-html="promo.notice_text_approved"></p>
						
					</div>
					<div class="col-sm-12">
						<hr>
					</div>
				</div>
                <div v-if="accessibilityData" style="padding-top:20px;"></div> 
                <div class="page_body description_text text_left" v-if="locale=='en-ca'" v-html="currentPage.body"></div>
                <div class="page_body description_text text_left" v-else v-html="currentPage.body_2"></div>
            </div>
        </div>
    </div>
</template>

<style>
    .page_title {
        /*border-top:1px solid #aea99e;*/
        border-bottom:1px solid #aea99e;
        height: 35px;
        line-height: 35px;
    }
    #pages_container img{
        width: 100%;
        height: auto;
    }
    .acc_title {
        margin-top:20px;
    }
    img {
        max-width: 100%;
    }
</style>

<script>
    define(["Vue", "vuex", "moment", "moment-timezone", "vue-moment", "bootstrap-vue"], function(Vue, Vuex, moment, tz, VueMoment, BootstrapVue) {
        Vue.use(BootstrapVue);
        return Vue.component("services-component", {
            template: template, // the variable template will be injected,
            data: function() {
                return {
                    success_subscribe: false,
                    currentPage: null,
                    pageBanner : null,
                    services : []
                }
            },
            props:['id', 'locale'],
            beforeRouteUpdate(to, from, next) {
                this.loadData().then(response => {
                    this.currentPage = response[0].data;
                    var temp_repo = this.findRepoByName('Services Banner');
                    if (temp_repo) {
                        this.pageBanner = temp_repo.images[0];
                    }
                    this.pageBanner = this.pageBanner;
                });
                next();
            },
            created(){
                this.loadData().then(response => {
                    this.currentPage = response[0].data;
                    var temp_repo = this.findRepoByName('Services Banner');
                    if(temp_repo) {
                        this.pageBanner = temp_repo.images[0];
                    }
                    this.pageBanner = this.pageBanner;
                });
            },
            watch: {
                currentPage () {
                    var temp_service = [];
                    _.forEach(this.currentPage.subpages, function(val, key) {
                        var temp_val = {};
                        temp_val.title = val.title;
                        temp_val.text  = val.body;
                        temp_val.is_visible = false;
                        if (key == 0) {
                          temp_val.is_visible = true;
                        }
                        temp_val.id = "accord_"+ (key+1);
                        temp_service.push(temp_val);
                    });
                    this.services = temp_service;
               }
            },
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'timezone',
                    'findRepoByName'
                ])
            },
            methods: {
                loadData: async function() {
                    try {
                        let results = await Promise.all([
                            this.$store.dispatch('LOAD_PAGE_DATA', { url: this.property.mm_host + "/pages/pinecentre-services-available.json" }),
                            this.$store.dispatch("getData", "repos")
                        ]);
                        return results;
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                }
            }
        });
    });
</script>