<template>
    <div class="sticky"><!--<div :class="{sticky : stickyMenu}">-->
    	<div class="top_bar">
    		<div class="site_container">
    			<div class="row top_bar_wrapper">
    				<div class="col-sm-6">
    					<div class="mobile_site_logo visible_phone">
    					    <router-link to="/"><img :src="property_logo" alt="Property Logo"/></router-link>
    				    </div>
    					<div id="home_hours_container" class="hidden_phone">
    						<p class="open_now"><span v-if="todays_hours.is_closed == null || !todays_hours.is_closed ">{{$t("header.open_today")}}</span><span v-else>{{$t("header.closed")}}</span> <span v-if="todays_hours.is_closed == null || !todays_hours.is_closed "><span style="margin:0 20px">|</span> {{todays_hours.open_time | moment("h:mma", timezone)}} - {{todays_hours.close_time | moment("h:mma", timezone)}} </span></p>
    					</div>
    				</div>
    				<div class="col-sm-6 hidden_phone text-right">
    					<div class="header_social">
    					    <social-links></social-links>
    					</div>
    					<router-link id="signup" to="/newsletter">{{$t("header.sign_up")}}</router-link>
    					<!--<span> <span @click="changeLocale('en-ca')"> en</span> | <span @click="changeLocale('fr-ca')">fr</span></span>-->
    				</div>
    				<div id="menu-icon" @click="show_mobile_menu = !show_mobile_menu" :class="{ open: show_mobile_menu}">
    					<span></span>
    					<span></span>
    					<span></span>
    					<span></span>
    				</div>
    				<div class="mobile_nav_container visible_phone">
    				    <transition name="custom-classes-transition" enter-active-class="animated slideInRight" leave-active-class="animated slideOutRight">
    						<nav id="mobile_nav" v-show="show_mobile_menu">
    							<ul>
    								<div class="mobile_menu_site_logo">
    									<router-link to="/"><img :src="property_logo" alt="Property Logo"/></router-link>
    								</div>
    								<li v-for="(item,key) in menu_items" class="menu_item">
    							        <router-link :to="item.href" v-if="item.sub_menu == undefined">{{$t(item.name)}}</router-link>
    							        <div v-else>
    							         <!--   <a href="#" @click="toggleSubMenu(item.name); item.show_sub_menu = !item.show_sub_menu">{{$t(item.name)}}</a> -->
    							         <!--   <transition name="custom-classes-transition"   enter-active-class="animated fadeIn" leave-active-class="animated fadeOut" :duration="{ leave: 700 }" >-->
    							         <!--       <ul v-if="item.sub_menu" v-show="item.show_sub_menu" :key="key">-->
        								 <!--           <li v-for="sub_menu in item.sub_menu" class="dropdown_item" >-->
        								 <!--               <router-link :to="sub_menu.href">{{$t(sub_menu.name)}}</router-link>-->
        								 <!--           </li>-->
        									<!--	</ul>-->
        									<!--</transition>-->
        									<b-card no-body class="mb-1">
                                                <b-card-header header-tag="header" class="p-1" role="tab">
                                                    <b-btn block @click="item.show_sub_menu = !item.show_sub_menu" :class="item.show_sub_menu ? 'collapsed' : null" :aria-controls="item.name" :aria-expanded="item.show_sub_menu ? 'true' : 'false'">
                                                        <i v-if="item.show_sub_menu"  class="fa fa-caret-down"></i>
                                                        <i v-else  class="fa fa-caret-right"></i>
                                                        {{$t(item.name)}}
                                                    </b-btn>
                                                </b-card-header>
                                                <b-collapse v-model="item.show_sub_menu" :id="item.name" :visible="item.show_sub_menu" :accordion="item.name" role="tabpanel" class="accordion_body">
                                                    <b-card-body v-for="sub_menu in item.sub_menu">
                                                        <p class="card-text">{{sub_menu.name}}</p>
                                                    </b-card-body>
                                                </b-collapse>
                                            </b-card>
    							        </div>
    							        
    							    </li>
    							</ul>
    							<div class="small_hr"></div>
    							<div class="tel_num" v-if="property">
                                    <a :href="'tel:'+property.contact_phone">{{property.contact_phone}}</a>
                                </div>
                                <div>
                                   <p style="display:block"> {{property.address1}}</p>
                                    <p style="display:block">{{property.city}}, {{property.postal_code}} {{property.province_state}}</p>
                                </div>
    							<div class="header_social">
    							    <social-links></social-links>
    							</div>
    							<div class="small_hr"></div>
    						</nav>
    					</transition>
    				</div>
    			</div>
    		</div>
    	</div>
    	<div class="menu_bar hidden_phone">
    		<div class="site_container">
    			<div class="nav_container hidden_phone">
    				<div class="site_logo">
    					<router-link to="/"><img :src="property_logo" alt="Property Logo"/></router-link>
    				</div>
    				<div class="row top_nav hidden_phone">
    					<nav id="primary_nav">
    						<ul>
    						    <li v-for="item in menu_items" class="menu_item">
    						        <router-link :to="item.href">{{$t(item.name)}}</router-link>
    						        <ul v-if="item.sub_menu">
    						            <li v-for="sub_menu in item.sub_menu" class="dropdown_item">
    						                <router-link :to="sub_menu.href">{{$t(sub_menu.name)}}</router-link>
    						            </li>
    								</ul>
    						    </li>
    						</ul>
    					</nav>
    				</div>
    			</div>
    		</div>
    	</div>
    </div>
</template>

<script>
    define(["Vue", "vuex", 'vue!social_links.vue', "bootstrap-vue"], function (Vue, Vuex, SocialLinks, BootstrapVue) {
        Vue.use(BootstrapVue);
        return Vue.component("header-component", {
            template: template, // the variable template will be injected,
            data: function() {
                return {
                    dataLoaded: false,
                    show_mobile_menu: false,
                    property_logo: "//codecloud.cdn.speedyrails.net/sites/5a9714046e6f644dc3160000/image/png/1520457420000/whitelogo1@2x.png",
                    menu_items: [
                        {
                            name: "header.stores",
                            href: "/stores",
                            show_sub_menu: false,
                            sub_menu: [{
                                    name: "header.stores",
                                    href: "/stores"
                                },
                                {
                                    name: "header.centre_map",
                                    href: "/map"
                                },
                                {
                                    name: "header.restaurants",
                                    href: "/restaurants"
                                }
                            ]
                        },
                        {
                            name: "header.promos",
                            href: "/promotions",
                            show_sub_menu: false,
                        },
                        {
                            name: "header.news_events",
                            href: "/events",
                            show_sub_menu: false,
                            sub_menu: [{
                                    name: "header.events",
                                    href: "/events"
                                },
                                {
                                    name: "header.blog",
                                    href: "/blog"
                                }
                            ]
                        },
                        {
                            name: "header.services",
                            href: "/pages/bramaleacitycentre-services",
                            show_sub_menu: false,
                            sub_menu: [{
                                    name: "header.services ",
                                    href: "/pages/bramaleacitycentre-services"
                                },
                                {
                                    name: "header.gift_card",
                                    href: "/pages/bramaleacitycentre-gift-cards"
                                },
                                {
                                    name: "header.accessibility",
                                    href: "/pages/bramaleacitycentre-accessibilty"
                                }
                                
                            ]
                        },
                        {
                            name: "header.contact_us",
                            href: "/contact_us",
                            show_sub_menu: false,
                            sub_menu: [
                                {
                                    name: "header.contact_us",
                                    href: "/contact_us"
                                },
                                {
                                    name: "header.hours",
                                    href: "/hours"
                                },
                                {
                                    name: "header.employment",
                                    href: "/jobs"
                                },
                                {
                                    name: "header.leasing",
                                    href: "/pages/bramaleacitycentre-leasing"
                                },
                                {
                                    name: "header.newsletter",
                                    href: "/newsletter"
                                }
                            ],
                        }
                    ],
                }
            },
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'timezone',
                    'banners',
                    'hours',
                    'getTodayHours',
                    'processedStores',
                ]),
                todays_hours() {
                    return this.getTodayHours;
                },
                allStores() {
                    return this.processedStores;
                }
            },
        });
    });
</script>