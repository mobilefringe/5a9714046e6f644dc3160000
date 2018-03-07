<template>
    <a href="https://www.instagram.com/shopbdc_" target="_blank"><i class="fa fa-instagram" aria-hidden="true"></i></a>
    <a href="https://www.facebook.com/bonniedoonsc" target="_blank"><i class="fa fa-facebook-official" aria-hidden="true"></i></a>
    <a href="https://www.pinterest.ca/bonniedoonsc" target="_blank"><i class="fa fa-pinterest" aria-hidden="true"></i></a>
    <a href="https://www.twitter.com/bonniedoonsc" target="_blank"><i class="fa fa-twitter" aria-hidden="true"></i></a>
</template>
<script>
    define(["Vue", "vuex", "moment", "moment-timezone", "vue-moment"], function(Vue, Vuex, moment, tz, VueMoment) {
        return Vue.component("page-details-component", {
            template: template, // the variable template will be injected,
            data: function() {
                return {
                    success_subscribe: false,
                    currentPage: null,
                    pageBanner : null
                }
            },
            props:['id', 'locale'],
            beforeRouteUpdate(to, from, next) {
                this.loadData(to.params.id).then(response => {
                    this.currentPage = response[0].data;
                    var temp_repo = this.findRepoByName('Pages Banner');
                    if(temp_repo) {
                        this.pageBanner = temp_repo.images[0];
                    }
                    this.pageBanner = this.pageBanner;
                });
                next();
            },
            created(){
                this.loadData(this.id).then(response => {
                    this.currentPage = response[0].data;
                    var temp_repo = this.findRepoByName('Pages Banner');
                    if(temp_repo) {
                        this.pageBanner = temp_repo.images[0];
                    }
                    this.pageBanner = this.pageBanner;
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
                loadData: async function(id) {
                    try {
                        // avoid making LOAD_META_DATA call for now as it will cause the entire Promise.all to fail since no meta data is set up.
                        console.log("this.id", this.id);
                        let results = await Promise.all([this.$store.dispatch('LOAD_PAGE_DATA', {url: this.property.mm_host + "/pages/" + id + ".json"}),this.$store.dispatch("getData", "repos")]);
                        return results;
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                },
            }
        });
    });
</script>