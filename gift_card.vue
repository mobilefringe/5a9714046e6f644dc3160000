<template>
	<div v-if="dataloaded">
		<div class="page_header" v-if="promoBanner" v-lazy:background-image="promoBanner.image_url">
			<!--http://via.placeholder.com/1920x300-->
			<div class="site_container">
				<div class="header_content caps">
					<h1>Looking for the Perfect Gift?</h1>
					<h2 style="display:none;">Scroll to view events</h2>
					<h3 style="display:none;">View all events below</h3>
				</div>
			</div>
		</div>
		<div class="site_container page_content">
			<div id="events_container" v-if="news.length > 0">
				<paginate name="news" v-if="news" :list="news" class="paginate-list margin-60" :per="4">
					<div class="row event_container" v-for="(item, index) in paginated('news')" :class="{ 'last': index === (paginated('news').length - 1) }">
						<div class="col-sm-6 col-md-4 event_image_container">
							<img :src="item.image_url"  class="event_image image" alt="item.title" />
						</div>
						<div class="col-sm-6 col-md-8 event_dets_container">
							<h4 class="event_name caps" v-if="locale=='en-ca'">{{ item.title }}</h4>
							<!--<h4 class="event_name caps" v-else>{{promo.name_2}}</h4>-->
							<!--<div class="event_thick_line"></div>-->
							<p class="event_dates">{{item.publish_date | moment("MMM DD, YYYY", timezone)}}</p>
							<p class="event_dates">By: {{item.author}}</p>
							<p class="event_desc" v-if="locale=='en-ca'">{{item.body_short}}</p>
							<!--<p class="event_desc" v-else>{{promo.description_short_2}}</p>-->
							<div class="text-right  col-sm-6" v-if="item" style="padding:0">
    							<router-link :to="{ name: 'newsDetails', params: { id: item.slug }}" class="event_learn_more pull-left">
    							    {{ $t("news_page.read_more") }} <i class="fa fa-angle-right" aria-hidden="true"></i>
    						    </router-link>
							</div>
						</div>
						<div class="col-sm-12">
							<hr>
						</div>
					</div>
				</paginate>
			</div>
			<div id="no_events" class="row" v-else>
				<div class="col-md-12">
					<p>{{$t("news_page.no_news_message")}}!</p>
				</div>
			</div>
			<div class="row margin-60">
				<div class="col-md-12">
					<paginate-links for="news" :async="true" :limit="5" :show-step-links="true"></paginate-links>
					<!--<paginate-links for="currentSelection" :async="true" :simple="{ next: 'Next »', prev: '« Back' }"></paginate-links>-->
				</div>
			</div>
		</div>
	</div>
</template>

<script>
    define(["Vue", "vuex", "moment", "moment-timezone", "vue-moment", "vue-meta", "vue-lazy-load", "vue-paginate", "vue-social-sharing"], function(Vue, Vuex, moment, tz, VueMoment, Meta, VueLazyload, VuePaginate, SocialSharing) {
        Vue.use(Meta);
        Vue.use(VueLazyload);
        Vue.use(VuePaginate);
        return Vue.component("gift-cards-component", {
            template: template, // the variable template will be injected
            props:['locale'],
            data: function() {
                return {
                    dataloaded: false,
                    promoBanner: null,
                    paginate: ['news'],
                }
            },
            created() {
                this.loadData().then(response => {
                    this.posts
                    this.dataloaded = true;
                    
                    var temp_repo = this.findRepoByName('Pages Banner');
                    if(temp_repo) {
                        this.promoBanner = temp_repo.images[0];
                    }
                });
            },
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'timezone',
                    'processedEvents',
                    'findRepoByName',
                    'news',
                    'findBlogByName'
                ]),
                news() {
                    var news = this.findBlogByName("News").posts;
                    var vm = this;
                    var temp_news = [];
                    _.forEach(news, function(value, key) {
                        today = moment().tz(vm.timezone);
                        webDate = moment(value.publish_date).tz(vm.timezone);
                        if (today >= webDate) {
                            if (_.includes(value.image_url, 'missing')) {
                                value.image_url = "//codecloud.cdn.speedyrails.net/sites/5c0581a36e6f643f53050000/image/jpeg/1527006352000/bccblogplaceholder.jpg";
                            }
                            value.body_short = _.truncate(value.body, { 'length': 99, 'separator': ' ' });
                            
                            temp_news.push(value);
                        }
                    });
                    news = _.reverse(_.sortBy(temp_news, function (o) { return o.publish_date }));
                    console.log(news)
                    return news
                },
            },
            methods: {
                loadData: async function() {
                    try {
                        // avoid making LOAD_META_DATA call for now as it will cause the entire Promise.all to fail since no meta data is set up.
                        let results = await Promise.all([this.$store.dispatch("getData", "blogs"), this.$store.dispatch("getData", "repos")]);
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                },
                shareURL(slug){
                    var share_url = "http://bramaleacitycentre.com/news/" + slug;
                    return share_url;
                },
            }
        });
    });
</script>