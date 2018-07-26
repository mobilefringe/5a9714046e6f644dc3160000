<template>
	<div class="page_container" id="contact_us_container">
		<!-- for some reason if you do not put an outer container div this component template will not render -->
		<div  v-if="pageBanner" class="page_header" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
			<div class="site_container">
				<div class="header_content"></div>
			</div>
		</div>
		<div class="site_container">
			<div class="row margin_40">
			    <div class="col-md-12">
			        <div class="center" v-if="para1" v-html="para1.body"></div>
			    </div>
			</div>
			<div class="row margin_40">
			    <div class="col-md-12">
			        <a href="https://bccfashionicity.com/site/register" target="_blank">
			            <div class="contact_btn">Join Now</div>
			        </a>
			    </div>
		    </div>
			<div class="row margin_40">
			    <div class="col-md-12">
			        <div class="center" v-if="para2" v-html="para2.body"></div>
			    </div>
			</div>
			<div class="row margin_40">
			    <div class="col-md-3"></div>
			    <div class="col-md-3">
			        <a href="https://play.google.com/store/apps/details?id=ca.wearecircus.bcc" target="_blank">
			            <img class="rewards_badge" src="//codecloud.cdn.speedyrails.net/sites/5afb8d456e6f6436aa100000/image/png/1532627380000/googleplay.png" alt="Google Play Store" />
			        </a>
			    </div>
			    <div class="col-md-3">
			        <a href="https://itunes.apple.com/ca/app/bramalea-city-centre/id1042094590?mt=8" target="_blank">
			            <img class="rewards_badge" src="//codecloud.cdn.speedyrails.net/sites/5afb8d456e6f6436aa100000/image/png/1532627378000/applestore.png" alt="Apple App Store" />
			        </a>    
			    </div>
			    <div class="col-md-3"></div>
			</div>
			<div class="padding_top_40"></div>
		</div>
	</div>
</template>
<style>
    a:hover {
        text-decoration: none;
    }
    .contact_btn {
        margin: 0 auto;
        max-width: 250px;
        display: block;
    }
</style>
<script>
    define(["Vue", "vuex"], function(Vue, Vuex) {
        return Vue.component("rewards-component", {
            template: template, // the variable template will be injected
            data: function() {
                return {
                    pageBanner: null,
                    para1: null,
                    para2: null
                }
            },
            created () {
                this.loadData().then(response => {
                    var temp_repo = this.findRepoByName('FashioniCity Banner');
                    if(temp_repo) {
                        this.pageBanner = temp_repo.images[0];
                    }

                    this.para1 = response[1].data;
                    this.para2 = response[1].data.subpages[0];
                });    
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
                        let results = await Promise.all([this.$store.dispatch("getData", "repos"), this.$store.dispatch('LOAD_PAGE_DATA', {url: this.property.mm_host + "/pages/bramaleacitycentre-fashionicity.json"})]);
                        return results;
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                },
            }
        });
    });
</script>
