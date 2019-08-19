<template>
    <div class="star" :class="starType">
        <span v-for="item in itemClass" :class="item" class="star-item" :key="item.id"></span>
    </div>
</template>
<script>
const length = 5
const cls_on = 'on'
const cls_off = 'off'
const cls_half = 'half'
export default {
    props: {
        size: {
            type: Number
        },
        score: {
            type: Number
        }
    },
    computed: {
        starType() {
            return "star" + this.size
        },
        itemClass() {
            var arr = []
            var num = Math.floor(this.score * 2)/2
            var cOff = num % 1 !== 1
            var cOn = Math.floor(this.score)
            for(var i = 0; i < cOn; i++) {
                arr.push(cls_on)
            }
            if(cOff){
                arr.push(cls_off)
            }
            while(arr.legnth < length){
                arr.push(cls_half)
            }
            return arr
        }
    }
}
</script>
<style lang="stylus">
@import '../../common/stylus/mixin.styl'

    .star
        .star-item
            display: inline-block
        &.star24
            .star-item
                width: 10px
                height: 10px
                background-size:10px
                &.on
                    bg-image('star24_on')
                &.off
                    bg-image('star24_off')
                &.half
                    bg-image('star24_half')
        &.star36
            .star-item
                width: 15px
                height: 15px
                background-size:15px
                &.on
                    bg-image('star24_on')
                &.off
                    bg-image('star24_off')
                &.half
                    bg-image('star24_half')
        &.star48
            :first-child
                margin-left: 0
            .star-item
                width: 20px
                height: 20px
                background-size:20px
                margin-right: 10px
                &.on
                    bg-image('star24_on')
                &.off
                    bg-image('star24_off')
                &.half
                    bg-image('star24_half')
</style>
