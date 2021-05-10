<template>
    <div>
        <div class="tracker">
            <h1>IP Address Tracker</h1>
            <div class="search">
                <form @submit.prevent="track(ipNew)">
                    <input type="text" v-model="ipNew" placeholder="Search for any IP address">
                    <button @click="track(ipNew)"><img src="../assets/icon-arrow.svg" alt=""></button>
                </form>
            </div>
        </div>
        <Result 
            :ipaddress="ipaddress ? ipaddress : ipInitial" 
            :city="city" 
            :country="country" 
            :postalCode="postalCode" 
            :timezone="timezone" 
            :isp="isp" 
        />
        <div class="empty-map" v-if="loading">
            <p>{{ errors }}</p>
        </div>
        <div v-else>
            <Map :lat="lat" :lng="lng" v-if="lat"/>
            <div class="empty-map" v-else>
                <p>{{ errors }}</p>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import Result from './Result'
    import Map from './Map.vue'

    export default {
        name: "Search",
        components: {
            Result,
            Map
        },
        data() {
            return{
                ipInitial: "000.000.000.00",
                ipNew: "",
                ipaddress: "",
                city: "...",
                country: "",
                postalCode: "",
                timezone: "00:00",
                isp: "...",
                lat: null,
                lng: null,
                loading: true,
                errors: ""
            }
        },
        methods: {
            track: function(value){
                this.loading = true
                this.errors = "Loading..."
                if (/^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/.test(value)) {    
                    axios.get(`https://geo.ipify.org/api/v1?apiKey=at_B441eB7OQjQpzhCCTSeaU2HxBDHSD&ipAddress=` + value)
                    .then(response => {
                        this.ipaddress = response.data.ip
                        this.city = response.data.location.city 
                        this.country = response.data.location.country
                        this.postalCode = response.data.location.postalCode
                        this.timezone = response.data.location.timezone
                        this.lat = response.data.location.lat
                        this.lng = response.data.location.lng
                        this.isp = response.data.isp
                        this.loading = false
                    })
                    .catch(e => {
                        this.errors = "Sorry! This website has reached the search limit."
                    })
                }else{
                    this.errors = "Please enter a valid IP address."
                }
            }
        },
        beforeMount(){
            axios.get('https://api.ipify.org?format=json')
            .then(response => {
                this.ipInitial = response.data.ip;
                this.track(this.ipInitial);
                this.loading = false
            })
            .catch(e => {
                this.errors = "Sorry, an adblocker issue occurred."
            });
        }
    };  
</script>

<style scoped lang="scss" scope>
    @import "./../main.scss";
    .tracker{
        height: 220px;
        background-image: url('../assets/pattern-bg.png');
        background-size: cover;
        background-repeat: no-repeat;
        padding: 30px 0;
        h1{
            color: #fff;
            font-size: 1.58rem;
            font-weight: 500;
            text-align: center;
            margin: 0;
        }
        .search{
            display: flex;
            justify-content: center;
            margin: auto;
            padding: 27px 0;
            form{
                width: auto;
                position: relative;
                input[type="text" i]{
                    width: 498px;
                    height: 56px;
                    border-radius: 15px;
                    border: 0;
                    padding: 0 27px;
                    font-size: 1rem;
                    border: 1px solid #fff;
                    &:focus{
                        outline: 0;
                        border: 1px solid $dark-gray;
                    }
                }
                button{
                    position: absolute;    
                    top: 0;
                    right: 0px;
                    width: 57px;
                    height: 58px;
                    border-radius: 0 15px 15px 0;
                    border: 0;
                    background: #000;
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    cursor: pointer;
                    &:hover{
                        background: $dark-gray;
                    }
                    &:focus{
                        outline: 0;
                        border: 1px solid #000;
                        background: $dark-gray;
                    }
                }
            }
        }
        @media (max-width: 767.98px) {
             .search{
                display: block;
                overflow: hidden;
                width: 90%;
                form{
                    input[type="text" i]{
                        width: calc(100% - 100px);
                    }
                }
            }
        }
    }
    .empty-map{
        background: #aad3df;
        height: calc(100vh - 280px);
        display: flex;
        p{
            margin: auto;
            display: flex;
            align-content: center;
            text-align: center;
        }
    }
    @media (max-width: 767.98px) {
        .tracker{
            height: 300px;
            .search{
                display: block;
                overflow: hidden;
                width: 90%;
                form{
                    input[type="text" i]{
                        width: calc(100% - 100px);
                    }
                }
            }
        }
    }
    @media (max-width: 575.98px) { 
        .tracker{
            h1{
                font-size: 1rem;
            }
        }
    }
</style>
