<template>
    <div>
        <home-header>
        </home-header>
        <home-swiper :list='swiperList'>
        </home-swiper>
        <home-icons :list='iconList'>
        </home-icons>
        <home-recommend :list='recommendList'>
        </home-recommend>
        <home-weekend :list='weekendList'>
        </home-weekend>
    </div>
</template>

<script>
// 引入header组件
    import homeHeader from './components/homeHeader.vue'
    import homeSwiper from './components/swiper.vue'
    import homeIcons from './components/icons.vue'
    import homeRecommend from './components/recommend.vue'
    import homeWeekend from './components/weekend.vue'
    import axios from 'axios'
    import { mapState } from 'vuex'
    export default {
        name: 'Homepage',
        components: {
            homeHeader,
            homeSwiper,
            homeIcons,
            homeRecommend,
            homeWeekend
        },
        data () {
            return {
                swiperList: [],
                iconList: [],
                recommendList: [],
                weekendList: [],
                lastCity: ''
            }
        },
        methods: {
            // 执行axios函数
            getHomeInfo () {
                axios.get('/api/index.json?city=' + this.city)
                    .then(this.getHomeInfoSucc)
            },
            getHomeInfoSucc (res) {
                res = res.data
                if (res.ret && res.data) {
                    const data = res.data
                    this.swiperList = data.swiperList
                    this.iconList = data.iconList
                    this.recommendList = data.recommendList
                    this.weekendList = data.weekendList
                }
            }
        },
        computed: {
            ...mapState(['city'])
        },
        // mounted指页面挂载完成之后执行
        mounted () {
            this.getHomeInfo()
            this.lastCity = this.city
        },
        activated () {
            if (this.lastCity !== this.city) {
                this.lastCity = this.city
                this.getHomeInfo()
            }
        }
    }
</script>
<style>

</style>

