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
			<div class="row margin_40">
				<div class="col-md-6">
				    <iframe width="100%" height="500px" src="https://www.youtube.com/embed/ARL5pFDEZCw" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>	
				</div>
				<div class="col-md-6">
				    <img class="img-responsive" src="https://www.mallmaverick.com/system/site_images/photos/000/041/211/original/image_(1).png?1532099367" alt="">	
				</div>
			</div>
			<div class="row">
			    <div class="col-md-12">
			        <div v-if="currentPage" v-html="currentPage.body"></div>
			    </div>
			</div>
			<div class="padding_top_40"></div>
		</div>
	</div>
</template>

<script>
var _extends=Object.assign||function(e){for(var t=1;t<arguments.length;t++){var r=arguments[t];for(var n in r)Object.prototype.hasOwnProperty.call(r,n)&&(e[n]=r[n])}return e};define(["Vue","vuex"],function(e,t){return e.component("bees-component",{template:template,data:function(){return{currentPage:null,pageBanner:null}},created:function(){var e=this;this.loadData().then(function(t){var r=e.findRepoByName("Bees Banner");r&&(e.pageBanner=r.images[0]),e.currentPage=t[1].data})},computed:_extends({},t.mapGetters(["property","timezone","findRepoByName"])),methods:{loadData:function(){var e;return regeneratorRuntime.async(function(t){for(;;)switch(t.prev=t.next){case 0:return t.prev=0,t.next=3,regeneratorRuntime.awrap(Promise.all([this.$store.dispatch("getData","repos"),this.$store.dispatch("LOAD_PAGE_DATA",{url:this.property.mm_host+"/pages/bramaleacitycentre-bees-at-the-hive.json"})]));case 3:return e=t.sent,t.abrupt("return",e);case 7:t.prev=7,t.t0=t["catch"](0),console.log("Error loading data: "+t.t0.message);case 10:case"end":return t.stop()}},null,this,[[0,7]])}}})});
</script>
