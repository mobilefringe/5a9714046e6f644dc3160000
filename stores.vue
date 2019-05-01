<template>
	<div v-if="dataloaded">
		<div class="page_header" v-if="storeBanner" v-lazy:background-image="storeBanner.image_url">
			<div class="site_container">
				<div class="header_content">
					<h1>{{$t("stores_page.shopping")}}</h1>
				</div>
			</div>
		</div>
		<div class="site_container page_content">
			<div class="row bold">
				<div class="col-sm-6 col-md-4">
					<div class="store_search" >
						<search-component :list="allStores" :placeholder="$t('stores_page.find_your_store')" suggestion-attribute="name" v-model="search_result" @select="onOptionSelect" class="text-left">
							<template slot="item" scope="option" class="manual">
								<article class="media">
									<p>
										<strong>{{ option.data.name }}</strong>
									</p>
								</article>
							</template>
						</search-component>
						<img src="//codecloud.cdn.speedyrails.net/sites/5a6a54eb6e6f647da51e0100/image/png/1517497861636/search_icon_2x.png" class="pull-right" id="store_search_img" alt="search icon">
					</div>
				</div>
				<div class="col-sm-6 col-md-4">
					<div class="store_search" >
						<div class="category-select-container">
							<v-select v-model="selectedCat" :options="dropDownCats" :searchable="false" :on-change="filterByCategory" class="category-select" :placeholder="$t('stores_page.sort_by_cats')" id="selectByCat"></v-select>
						</div>
					</div>
				</div>
				<div class="col-md-4 col-sm-12 hidden_phone">
					<div class="store_search" >
						<router-link class="directory_link" to="/map">
							<div class="promotions_header_container directory_btn">{{$t("stores_page.view_map")}}</div>
						</router-link>
					</div>
				</div>
			</div>
			<div class="row">
				<div id="store_list_container">
				    <!--<store-masonry :filteredStores="filteredStores"></store-masonry>-->
				    <div v-masonry transition-duration="0.3s" item-selector=".stores-grid-item" horizontal-order="true">
                        <transition-group name="custom-classes-transition" enter-active-class="animated fade" leave-active-class="animated fade" tag="div">
                            <div v-masonry-tile  v-for="(store, index) in filteredStores"  v-if="showMore > index" :key="index" class="stores-grid-item">
                        	    <div class="store_logo_container">
                        	        <router-link :to="'/stores/'+ store.slug">
                            			<div v-if="!store.no_store_logo">
                            			    <img class="transparent_logo" src="//codecloud.cdn.speedyrails.net/sites/59347e776e6f64538f150000/image/png/1545150810975/default_background.png" alt="">
                            			    <img  class="store_img" :src="store.store_front_url_abs" :alt="'Click here to view info about ' + store.name +  store.id"/>
                            			</div>
                                        <div v-else class="no_logo_container">
                                            <img class="transparent_logo" src="//codecloud.cdn.speedyrails.net/sites/59347e776e6f64538f150000/image/png/1545150810975/default_background.png" alt="">
                                            <div class="no_logo_text">
                                                <div class="store_text"><h2>{{ store.name }}</h2></div>
                                            </div>
                                        </div>
                            			<div class="store_tag" v-if="store.total_published_promos">
                							<div class="store_tag_text">Promotion</div>
                						</div>
                						<div class="store_tag" v-if="!store.total_published_promos && store.is_coming_soon_store">
                							<div class="store_tag_text">Coming Soon</div>
                						</div>
                						<div class="store_tag" v-if="!store.total_published_promos && !store.is_coming_soon_store && store.is_new_store">
                							<div class="store_tag_text">New Store</div>
                						</div>
                						<div class="store_details">
                						    <div class="store_text"><h2>{{ store.name }}</h2></div>    
                						</div>
                            		</router-link>
                        	    </div>
                            </div>
                        </transition-group>
                    </div>
				</div>
			</div>
			<div class="load_more animated_btn swing_in" v-if="filteredStores && showMore <= filteredStores.length" @click ="loadMore()">
                        <p>Load More</p>
                    </div>
		</div>
	</div>
</template>

<script>
    define(["Vue", "vuex", "vue-select", "jquery", "smooth-zoom", "vue!png-map", "vue!search-component", "vue-lazy-load", "vue!store-masonry"], function(Vue, Vuex, VueSelect, $, smoothZoom, PNGMapComponent, SearchComponent, VueLazyload, StoreMasonry) {
        Vue.use(VueLazyload);
        return Vue.component("stores-component", {
            template: template, // the variable template will be injected
            data: function() {
                return {
                    selectedCat: null,
                    filteredStores: null,
                    dataloaded: false,
                    storeBanner : null,
                    search_result : null,
                    showMore: 18,
                    incrementBy: 18
                }
            },
            created (){
                this.loadData().then(response => {
                    this.dataloaded = true;
                    this.filteredStores = this.allStores;
                    
                    var temp_repo = this.findRepoByName('Stores Banner');
                    if (temp_repo) {
                        this.storeBanner = temp_repo.images[0];
                    }
                });
            },
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'timezone',
                    'processedStores',
                    'processedCategories',
                    'storesByAlphaIndex',
                    'storesByCategoryName',
                    'findCategoryById',
                    'findCategoryByName',
                    'findRepoByName'

                ]),
                allStores() {
                    var stores = this.processedStores;
                    stores.map(store => {
                        if (_.includes(store.store_front_url_abs, 'missing')) {
                           store.no_store_logo = true;
                        } else {
                          store.no_store_logo = false;
                        }
                    });
                    return this.processedStores;
                },
                allCatergories() {
                    return this.processedCategories;
                },
                dropDownCats() {
                    var cats = _.map(this.processedCategories, 'name');
                    cats.unshift('All');
                    return cats;
                },
                filterStores() {
                    letter = this.selectedAlpha;
                    if (letter == "All") {
                        this.filteredStores = this.allStores;
                    } else {
                        var filtered = _.filter(this.allStores, function(o, i) {
                            return _.lowerCase(o.name)[0] == _.lowerCase(letter);
                        });
                        this.filteredStores = filtered;
                    }
                },
                filterByCategory() {
                    category_id = this.selectedCat;
                    if (category_id == "All" || category_id == null || category_id == undefined) {
                        category_id = "All";
                    } else {
                        category_id = this.findCategoryByName(category_id).id;
                    }

                    if (category_id == "All") {
                        this.filteredStores = this.allStores;
                    } else {

                        var find = this.findCategoryById;
                        var filtered = _.filter(this.allStores, function(o) {
                            return _.indexOf(o.categories, _.toNumber(category_id)) > -1;
                        });
                    
                        this.filteredStores = filtered;
                    }
                    var el = document.getElementById("selectByCat");
                    if(el) {
                        el.classList.remove("open");
                    }
                    
                }
            },
             methods: {
                loadData: async function() {
                    try {
                        let results = await Promise.all([
                            this.$store.dispatch("getData", "categories"), 
                            this.$store.dispatch("getData", "repos")
                        ]);
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                },
                loadMore() {
                    if (this.showMore <= this.filteredStores.length) {
                        var num = this.showMore + this.incrementBy;
                        this.showMore = num;
                    }
                },
                onOptionSelect(option) {
                    this.search_result = "";
                    this.$router.push("/stores/"+option.slug);
                }
            }
        });
    });
</script>