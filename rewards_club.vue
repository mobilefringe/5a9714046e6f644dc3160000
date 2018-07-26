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
			        <div v-if="currentPage" v-html="currentPage.body"></div>
			    </div>
			</div>
			<div class="row margin_40">
			    <div class="col-md-12">
			        <div class="contact_btn">Join Now</div>
			    </div>
		    </div>
			<div class="row margin_40">
			    <div class="col-md-12">
			        <div v-if="currentPage" v-html="currentPage.body"></div>
			    </div>
			</div>
			<div class="padding_top_40"></div>
		</div>
	</div>
</template>

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
                    console.log(response)
                    this.currentPage = response[1].data
                    
                    this.para1 = response[1].data;
                    this.para2 = response[1].data.subpages
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
