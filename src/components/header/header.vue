<template>
    <div class="header">
        <div class="nav">
            <img :src="seller.avatar" alt="">
            <div class="nav-b">
                <div class="centent-a">
                    <span class="item-a"></span>  <span class="item-b">{{seller.name}}</span>
                </div>
                <div class="centent-b">
                    <span class="item-b">{{seller.description}}</span>
                </div>
                <div class="centent-c" v-if='seller.description'>
                    <span class="item-a" :class="classMap[seller.supports[0].type]"></span><span class="item-b">{{seller.supports[0].description}}</span>
                </div>
            </div>
            <div class="privilege" @click="bagShow" v-if="seller.supports">
                <span> {{seller.supports.length}} 个</span><span class="icon-keyboard_arrow_right"></span>
            </div>
        </div>
        <div class="notice">
            <span class="notice-a">{{}}</span>
            <span class="notice-b">{{seller.bulletin}}</span>
            <i class="icon-keyboard_arrow_right"></i>
        </div>
        <div class="box">
            <img :src="seller.avatar" alt="">
        </div>
        <div v-show='flag' class="background">
            <div class="background-a clearfix">
                <div class="bag">
                    <h2>{{seller.name}}</h2>
                    <star class="star" :score= 'seller.score' :size = '48'></star>
                    <separate :text="favorable" class="favorable"></separate>
                    <div class="info" v-if="seller.supports">
                        <ul>
                            <li v-for="item in seller.supports" :key="item.type"><span :class="classMap[item.type]" class="map"></span><span class="map-a">{{item.description}}</span></li>
                        </ul>
                    </div>
                    <separate :text="notice" class="notices"></separate>
                    <p class="not-a">{{seller.bulletin}}</p>
                </div>
            </div>
            <div class="background-b">
                <i class="icon-close" @click="isflag"></i>
            </div>
        </div>
    </div>
</template>
<script>
import star from '../star/star.vue'
import separate from '../separate/separate.vue'
export default {
    props: {
        seller: {
            type: Object
        }
    },
    created() {
        this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
    },
    data () {
        return {
            flag : false,
            favorable: '优惠信息',
            notice:'商家公告'
        }
    },
    methods: {
        isflag() {
            this.flag = false
        },
        bagShow() {
            this.flag = true
        }
    },
    components: {
        star,
        separate
    }
}
</script>
<style lang="stylus" scoped>
@import '../../common/stylus/mixin.styl'

.header
    background-color: rgba(7,17,27,0.5)
    position: relative
    .nav
        display: flex
        padding: 24px 12px 18px 24px
        img
            width: 64px
            height: 64px
        .nav-b
            margin-left: 16px
            .centent-a
                margin-top: 2px
                .item-a
                    width:30px
                    height: 18px
                    display: inline-block
                    vertical-align: top
                    background-size: 30px 18px
                    bg-image(brand)
                .item-b
                    font-size: 16px
                    color: rgb(255,255,255)
                    font-weight: bold
                    line-height: 18px
            .centent-b
                margin-top:4px
                .item-b
                    font-size: 10px
                    color: rgb(255,255,255)
                    font-weight: bold
                    line-height: 12px
            .centent-c
                margin-top: 5px
                .item-a
                    width: 12px
                    height: 12px
                    display: inline-block
                    background-size: 12px
                    &.decrease
                        bg-image('decrease_2')
                    &.discount
                        bg-image('discount_2')
                    &.guarantee
                        bg-image('guarantee_2')
                    &.invoice
                        bg-image('invoice_2')
                    &.special
                        bg-image('special_2')
                .item-b
                    font-size: 10px
                    color: rgb(255,255,255)
                    font-weight: 200
                    line-height: 12px
        .privilege
            width: 48px
            height: 24px
            font-size: 10px
            color: rgb(255,255,255)
            background-color: rgba(0,0,0,0.2)
            border-radius: 9px
            position: absolute
            right: 20px
            bottom: 45px
            text-align: center
            line-height: 24px
    .notice
        font-size: 10px
        color: rgb(255,255,255)
        height: 28px
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        background-color: rgba(7,17,27,0.2)
        .notice-a
            display:inline-block
            width: 22px
            height: 12px
            margin-left: 12px
            bg-image(bulletin)
            background-size: 22px
        .notice-b
            font-size: 10px
            color: rgb(255,255,255)
            font-weight: 200
            line-height: 28px
    .box
        width:100%
        height: 100%
        position:absolute
        left: 0
        bottom: 0
        z-index: -1
        img
            backdrop-filter: blur(10px)
            width:100%
            height:100%
    .background
        width: 100%
        height: 100%
        background-color:rgba(7,17,27,0.8)
        z-index: 111
        position:fixed
        left: 0
        top: 0
        .background-a
            width: 100%
            min-height: 100%
            .bag
                margin-top: 64px
                padding-bottom: 64px
                h2
                    font-size: 16px
                    font-weight: 700
                    color: rgb(255,255,255)
                    line-height: 16px
                    margin:auto
                    text-align: center
                .star
                    margin: 10px auto 0 auto
                    text-align: center
                .favorable
                    margin-top: 18px
                .info
                    width: 80%
                    margin: 0 auto
                    font-size: 12px
                    font-weight: 200
                    color: rgb(255,255,255)
                    line-height: 12px
                    margin-top: 20px
                    ul
                        li
                            margin-bottom: 12px
                            .map
                                width: 16px
                                height: 16px
                                display: inline-block
                                background-size: 16px
                                vertical-align: top
                                &.decrease
                                    bg-image('decrease_2')
                                &.discount
                                    bg-image('discount_2')
                                &.guarantee
                                    bg-image('guarantee_2')
                                &.invoice
                                    bg-image('invoice_2')
                                &.special
                                    bg-image('special_2')
                            .map-a
                                margin-left: 6px
                                vertical-align: top
                                line-height: 16px
                .notices
                    margin-top: 18px
                .not-a
                    width: 80%
                    margin: 20px auto 0 auto
                    font-size: 12px
                    line-height: 24px
                    color: rgb(255,255,255)
                    font-weight: 200
        .background-b
            font-size: 32px
            color:rgba(255,255,255,0.5)
            clear: both
            margin: -42px auto 0 auto
            text-align : center
</style>
