<template>
    <div class="flex flex-col w-full justify-center items-center">
        <Citynav class="flex items-center justify-center mt-0 h-16 m-2"/>
        <div class="w-full sm:mt-8 mt-4">
            <InfoCity :node="city"/>
        </div>
        {{city}}
    </div>
</template>

<script>
import Citynav from '@/components/header/Citynav.vue'
import InfoCity from '../components/CityInfo.vue'

import axios from 'axios'
    export default {
        name: 'City',
        components: {
            Citynav, InfoCity
        },
        data() {
            return {
                city: {},
            }
        },
        mounted() {
            this.getCity()
        },
        watch: {
            $route(to, from) {
                if(to.name == 'City'){
                    this.getCity()
                }
            }
        },
        methods: {
            async getCity(){
                this.$store.commit("setIsLoading", true)

                // const country_slug = this.$route.params.country_slug
                const city_slug = this.$route.params.city_slug
                
                await axios
                .get(`/api/v1/city-tree/detailed/info/${city_slug}`)
                .then(response => {
                    this.city = response.data
                })
                .catch(error => {
                    console.log(error)
                })
                
                this.$store.commit("setIsLoading", false)
                document.title = this.city.name + ' | Plory '
                
            },
        }
    }
</script>

<style lang="scss" scoped>

</style>