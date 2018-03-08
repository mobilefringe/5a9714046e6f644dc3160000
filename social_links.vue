<template>
    <div>
        <a href="https://www.facebook.com/BramaleaCityCentre" target="_blank"><i class="fa fa-facebook" aria-hidden="true"></i></a>
        <a href="https://www.twitter.com/BCCstyle" target="_blank"><i class="fa fa-twitter" aria-hidden="true"></i></a>
        <a href="https://www.instagram.com/bccstyle" target="_blank"><i class="fa fa-instagram" aria-hidden="true"></i></a>
        <a href="https://www.youtube.com/BramaleaCityCentre" target="_blank"><i class="fa fa-youtube" aria-hidden="true"></i></a>
        <a href="https://www.pinterest.ca/BCCStyle" target="_blank"><i class="fa fa-pinterest" aria-hidden="true"></i></a>
        <a href="https://plus.google.com/104350008624406111130/about" target="_blank"><i class="fa fa-google-plus" aria-hidden="true"></i></a>
        <a href="//codecloud.cdn.speedyrails.net/sites/5a9714046e6f644dc3160000/image/png/1520548054663/bccsnapcode.png" data-lightbox="Snapchat" :data-title="BCC Snapchat"> <i class="fa fa-snapchat-ghost"></i></a>
         
    </div>
    
</template>
<script>
    define(["Vue", "vuex", "lightbox"], function(Vue, Vuex, Lightbox) {
        Vue.use(Lightbox);
        return Vue.component("social-links", {
            template: template, // the variable template will be injected,
            // data: function() {
            //     return {
            //         success_subscribe: false,
            //         currentPage: null,
            //         pageBanner : null
            //     }
            // },
            // computed: {
            //     ...Vuex.mapGetters([
            //         'property',
            //         'timezone',
            //         'findRepoByName'
            //     ])
            // },
            // methods: {
            //     loadData: async function(id) {
            //         try {
            //             // avoid making LOAD_META_DATA call for now as it will cause the entire Promise.all to fail since no meta data is set up.
            //             console.log("this.id", this.id);
            //             let results = await Promise.all([this.$store.dispatch('LOAD_PAGE_DATA', {url: this.property.mm_host + "/pages/" + id + ".json"}),this.$store.dispatch("getData", "repos")]);
            //             return results;
            //         } catch (e) {
            //             console.log("Error loading data: " + e.message);
            //         }
            //     },
            // }
        });
    });
</script>