<template>
    <div class="container">
        <div class="result" v-if="errors">
            <p class="errors">{{ errors }}</p>
        </div>
        <div class="result" v-else>
            <div class="block">
                <p class="title">IP address</p>
                <p class="details">{{ ipaddress }}</p>
            </div>
            <div class="block">
                <p class="title">Location</p>
                <p class="details">
                    <span v-if="city">{{ city }}</span>
                    <span v-if="country">, {{ country }}</span>
                    <span v-if="postalCode">, {{ postalCode }}</span>
                </p>
            </div>
            <div class="block">
                <p class="title">Timezone</p>
                <p class="details" v-if="timezone">UTC {{ timezone }}</p>
            </div>
            <div class="block">
                <p class="title">ISP</p>
                <p class="details">{{ isp }}</p>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Result",
        props: {
            ipaddress: String,
            city: String,
            country: String,
            postalCode: String,
            timezone: String,
            isp: String,
            errors: String
        }
    };
</script>

<style lang="scss">
    @import "./../main.scss";
    .result {
        height: 162px;
        width: 1110px;
        margin: 0 auto;
        position: absolute;
        top: 200px;
        left: 0;
        right: 0;
        background: #fff;
        display: flex;
        border-radius: 15px;
        z-index: 999;
        .block{    
            margin: 35px 0;
            padding: 0 35px;
            border-left: 1px solid #eee;
            width: 20%;
            .title{
                color: $gray;
                margin: 2px 0;
                font-size: 0.6rem;
                font-weight: 500;
                letter-spacing: 1.6px;
                text-transform: uppercase;
            }
            .details{
                color: black;
                margin: 15px 0;
                font-size: 1.25rem;
                font-weight: 500;
                text-transform: uppercase;
            }
        }
        .errors{
            color: black;
            margin: auto;
            font-size: 1.25rem;
            font-weight: 500;
        }
    }
    @media (max-width: 1199.98px) {
        .result {
            width: 90%;
        }
    }
    @media (max-width: 991.98px) {
        .result {
            width: 90%;
            .block{
                .details{
                    font-size: 1rem;
                }
            }
        }
    }
    @media (max-width: 767.98px) {
        .result {
            height: auto;
            display: block;
            text-align: center;
            top: 180px;
            .block{
                width: auto;
                display: block;
                .details{
                    font-size: 1rem;
                }
            }
        }
    }
</style>
