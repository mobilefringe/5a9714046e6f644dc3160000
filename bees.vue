<template>
	<div class="page_container" id="contact_us_container">
		<!-- for some reason if you do not put an outer container div this component template will not render -->
		<div  v-if="pageBanner" class="page_header" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
			<div class="site_container">
				<div class="header_content">
					<h1>{{$t("newsletter_page.newsletter")}}</h1>
				</div>
			</div>
		</div>
		<div class="site_container">
			<div class="row">
				<div class="col-md-12 contact_contents">
					
				</div>
			</div>
			<div class="padding_top_40"></div>
		</div>
	</div>
</template>

<style>
    .form-group label {
        display: inline-block;
    }
    .checkbox {
        font-weight: normal;
            margin-left: 20px;
    }
</style>
<script>
    define(["Vue", "vuex", "moment", "moment-timezone", "vue-moment", "vue-meta", 'vee-validate', 'jquery', 'utility', 'campaignMonitor'], function(Vue, Vuex, moment, tz, VueMoment, Meta, VeeValidate, $, Utility, campaignMonitor) {
        Vue.use(Meta);
        Vue.use(VeeValidate);
        return Vue.component("bees-component", {
            template: template, // the variable template will be injected
            props: ['id'],
            data: function() {
                return {
                    currentPage: null,
                    form_data : {},
                    formSuccess : false,
                    formError: false,
                    pageBanner: null
                }
            },
            created () {
                this.loadData().then(response => {
                    var temp_repo = this.findRepoByName('Newsletter Banner');
                    if(temp_repo) {
                        this.pageBanner = temp_repo.images[0];
                    }
                });    
            },
            mounted () {
                this.form_data.email = this.$route.query.email;
                $("#newsletter_email").val(this.form_data.email);
                if(this.$route.query.success == 'success') {
                    
                    this.formSuccess = true;
                    this.$router.replace('/newsletter');
                }
            },
            watch : {
                $route () {
                    this.form_data.email = this.$route.query.email;
                    $("#newsletter_email").val(this.form_data.email);
                    if(this.$route.query.success == 'success') {
                        
                        this.formSuccess = true;
                    }
                }
            },
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'timezone',
                    'findRepoByName'
                ])
            },
            methods: {
                validateBeforeSubmit(form) {
                    this.$validator.validateAll().then((result) => {
                        if (result) {
                            let errors = this.errors;
                            
                            if(errors.length > 0) {
                                console.log("Error");
                                this.formError = true;
                            }
                            else {
                                form.preventDefault();
                                console.log("No Error", form);
                                var vm = this;
                                $.getJSON(
                                    form.target.action ,
                                    $(form.target).serialize(),
                                    function (data) {
                                    if (data.Status === 400) {
                                      vm.formError = true;
                                        console.log("ERROR");
                                    } else { // 200
                                        vm.formSuccess = true;
                                        console.log("SUCCESS");
                                    }
                                });
                                form.preventDefault();
                            }
                        }
                    })
                },
                loadData: async function() {
                    try {
                        // avoid making LOAD_META_DATA call for now as it will cause the entire Promise.all to fail since no meta data is set up.
                        let results = await Promise.all([,this.$store.dispatch("getData", "repos")]);
                        return results;
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                },
            }
        });
    });
</script>
