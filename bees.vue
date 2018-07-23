<template>
	<div class="page_container" id="contact_us_container">
		<!-- for some reason if you do not put an outer container div this component template will not render -->
		<div  v-if="pageBanner" class="page_header" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
			<div class="site_container">
				<div class="header_content">
					<h1>{{$t("header.bees")}}</h1>
				</div>
			</div>
		</div>
		<div class="site_container">
			<div class="row .margin_40">
				<div class="col-md-6">
				    <iframe width="100%" height="500px" src="https://www.youtube.com/embed/SQbqv4XmtRg" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>	
				</div>
				<div class="col-md-6">
				    <img class="img-responsive" src="https://www.mallmaverick.com/system/site_images/photos/000/041/211/original/image_(1).png?1532099367" alt="">	
				</div>
			</div>
			<div class="row">
			    <div class="col-md-12">
			        <div></div>
			    </div>
			</div>
			<div class="padding_top_40"></div>
		</div>
	</div>
</template>

<script>
    define(["Vue", "vuex"], function(Vue, Vuex) {
        return Vue.component("bees-component", {
            template: template, // the variable template will be injected
            data: function() {
                return {
                    currentPage: null,
                    pageBanner: null
                }
            },
            created () {
                this.loadData().then(response => {
                    var temp_repo = this.findRepoByName('Bees Banner');
                    if(temp_repo) {
                        this.pageBanner = temp_repo.images[0];
                    }
                    console.log(response)
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
                        let results = await Promise.all([this.$store.dispatch("getData", "repos"), this.$store.dispatch('LOAD_PAGE_DATA', {url: this.property.mm_host + "/pages/bramaleacitycentre-bees-at-the-hive.json"})]);
                        return results;
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                },
            }
        });
    });
</script>
