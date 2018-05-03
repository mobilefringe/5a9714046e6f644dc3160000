<template>
    <div v-masonry transition-duration="0.3s" item-selector=".grid-item" class="visible_phone" >
        <div v-masonry-tile class="item" >
            <div v-for="feature in mobile_feature_items" :class="'grid-item ' + feature.masonry_class ">
            	<div  :class="{ 'ih-item circle effect19' : feature.no_hover_class}"> 
            	<!--class="ih-item circle effect19"-->
            	     <a v-if="feature.do_anchor_tag" :href="feature.url" target="_blank">
            			<img :src="feature.image_url" alt="name">
            			<div class="info">
            				<div class="content">
            					<span v-if="feature.name.length > 0"><h3 v-if="locale=='en-ca'">{{feature.name}} </h3></span>
            					<span v-if="feature.name_2.length > 0"><h3 v-else> {{feature.name_2}} </h3></span>
            				</div>
            			</div>
            		</a>
            		<router-link :to="feature.url" v-else>
            			<img :src="feature.image_url" alt="name">
            			<div class="info">
            				<div class="content">
            					<span v-if="feature.name.length > 0"><h3 v-if="locale=='en-ca'">{{feature.name}} </h3></span>
            					<span v-if="feature.name_2.length > 0"><h3 v-else> {{feature.name_2}} </h3></span>
            				</div>
            			</div>
            		</router-link>
            	</div>
            </div>
        </div>
    </div>
</template>

<script>
    define(["Vue", "vuex", 'masonry' , 'vue-masonry-plugin'], function(Vue, Vuex, masonry, VueMasonryPlugin) {
        Vue.use(VueMasonryPlugin.default);
        return Vue.component("social-links", {
            template: template, // the variable template will be injected,
        });
    });
</script>