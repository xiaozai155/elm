<template>
    <div class="separate">
        <div class="line"></div>
        <div class="text">{{text}}</div>
        <div class="line"></div>
    </div>
</template>
<script>
export default {
    props: {
        text: {
            type: String
        }
    }
}
</script>
<style lang="stylus">
    .separate
        display: flex
        width: 80%
        margin: 0 auto
        .line
            flex: 1
            border-bottom: 1px solid rgba(255,255,255,0.3)
            position: relative
            top: -6px
        .text
            font-size:14px
            color:rgb(255,255,255)
            margin: 0 12px
</style>
