<template>
    <div class="goods">
        <div class="goods-left" ref="menu">
            <ul>
                <li class="left-a" v-for='(item,index) in goods' :key="item.id" :class="{'conent':currentIndex == index}" @click="selectMenu(index)">
                    <span>{{item.name}}</span>
                </li>
            </ul>
        </div>
        <div class="goods-right" ref="foods">
            <ul>
                <li v-for="item in goods" :key="item.id" class="classify food-item-hook" >
                    <h3 class="itemName">{{item.name}}</h3>
                    <ul class="item-a">
                        <li class="fooda" v-for="food in item.foods" :key="food.id" @click="seleFood(food)">
                            <div class="gr-img">
                                <img :src="food.icon" alt="">
                            </div>
                            <div class="content">
                                <h4>{{food.name}}</h4>
                                <p>{{food.description}}</p>
                                <div class="sell"><span class="sell-a">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span></div>
                                <div class="price">
                                    <span class="price-a">￥{{food.price}}</span><span class="price-b" v-if="food.oldPrice">￥{{food.oldPrice}}</span>
                                    <div class="cunts">
                                        <addshop :foods = 'food' ></addshop>
                                    </div>
                                </div>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <shopcart :minPrice='seller.minPrice' :deliveryPrice='seller.deliveryPrice' :selectFoods='selectFoods'></shopcart>
        <food :food = 'selectFood' ref="food"></food>
    </div>
</template>
<script>
var Err_Ok = 0
import BScroll from 'better-scroll';
import shopcart from '../shopcart/shopcart.vue';
import addshop from '../addshop/addshop.vue';
import food from '../food/food.vue';
export default {
    data() {
        return {
            goods: [],
            listHeight: [],
            scroll: 0,
            selectFood: {}
        }
    },
    props: {
        seller: {
            type: Object
        }
    },
    created() {
        this.$http.get('/api/goods').then(response => {
            var response = response.data
            if(response.errno === Err_Ok) {
                this.goods = response.data
                this.$nextTick(() => {
                    this._linkScroll() 
                    this._calculateHeight()
                })
            }
        })
    },
    computed: {
        currentIndex() {
            for (let i = 0; i < this.listHeight.length; i++) {
                let height1 = this.listHeight[i];
                let height2 = this.listHeight[i + 1];
                if (!height2 || (this.scroll >= height1 && this.scroll < height2)) {
                    return i;
                }
            }
            return 0
        },
        selectFoods() {
            let foods = [];
            this.goods.forEach(item => {
                item.foods.forEach(food => {
                    if (food.count) {
                        foods.push(food);
                    };
                });
            });
            return foods;
        }
    },
    methods : {
        seleFood (food, event) {
            this.selectFood = food;
            this.$refs.food.show();
        },
        selectMenu(index) {
            let lis = this.$refs.foods.getElementsByClassName('food-item-hook')
            let el = lis[index]
            this.foodScroll.scrollToElement(el,300)
        },
        _linkScroll() {
            this.menuScroll = new BScroll(this.$refs.menu, {
                click: true
            })
            this.foodScroll = new BScroll(this.$refs.foods, {
                click: true,
                probeType: 3
            })
            this.foodScroll.on('scroll', (pos) => {
                this.scroll = Math.abs(Math.round(pos.y))
            })
        },
        _calculateHeight() {
            this.listHeight = []
            let height = 0
            this.listHeight.push(height)
            let lis = this.$refs.foods.getElementsByClassName('food-item-hook')
            for(let i = 0; i < lis.length; i++) {
                let item = lis[i]
                height += item.clientHeight
                this.listHeight.push(height)
            }
        }
    },
    components: {
        shopcart,
        addshop,
        food
    }
}
</script>
<style lang="stylus">
@import '../../common/stylus/mixin.styl'

.goods
    display: flex
    position:absolute
    left: 0
    top:174px
    bottom: 48px
    overflow: hidden
    width: 100%
    .goods-left
        flex: 0 0 80px
        width: 80px
        background-color: #f3f5f7
        .left-a
            width: 56px
            height: 54px
            padding: 0 12px
            border-1px(rgba(7,17,27,0.1))
            display: table
            text-align: center
            &.conent{
                position:relative
                font-size:12px
                font-weight:700
                background:rgb(255,255,255)
                border-none()
                margin-top:-1px
                z-index:99
            }
            span
                display: table-cell
                font-size: 12px
                color: rgb(7,17,27)
                font-weight: 200
                line-height: 14px
                vertical-align: middle
                
    .goods-right
        flex: 1
        ul
            .classify
                .itemName
                    font-size: 12px
                    color: rgb(147,153,159)
                    line-height: 26px
                    background-color: #f3f5f7
                    border-left: 2px solid #d9dde1
                    padding-left: 6px
                .item-a
                    .fooda
                        display: flex
                        margin: 18px
                        border-1px(rgba(7,17,27,0.1))
                        position: relative
                        .gr-img
                            width: 57px
                            height: 57px
                            margin-right: 10px
                            img
                                width: 100%
                                height: 100%
                        .content
                            h4
                                font-size: 14px
                                color: rgb(7,17,27)
                                line-height: 14px
                                margin-top: 2px
                            p,
                            .sell
                                font-size: 10px
                                color: rgb(147,153,159)
                                line-height: 10px
                                margin-top: 8px
                                .sell-a
                                    margin-right: 4px
                            .price
                                margin-top: 8px
                                padding-bottom: 8px
                                .price-a
                                    font-size: 14px
                                    color: red
                                    font-weight: 700
                                    line-height: 24px
                                .price-b
                                    font-size: 10px
                                    color: rgb(147,153,159)
                                    font-weight: normal
                                    line-height: 24px
                                .cunts
                                    position: absolute
                                    right: 0
                                    bottom:15px
</style>
