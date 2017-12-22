<template>
    <div class="holiday-card">
        <template v-if="!didEnterPassword">
            <input type="text" name="password" placeholder="Enter Password" v-model="passwordInput" @keypress.enter="checkPassword">
        </template>
        <template v-if="didEnterPassword">
            <card-svg ref="svg"></card-svg>
        </template>
    </div>   
</template>

<script>
import CardSVG from '../assets/xmass-2017-01.svg'

export default {
    name: 'IndexPage',
    props: {},
    data() {
        return {
            password: 'mountains',
            passwordInput: null,
            didEnterPassword: false,
        }
    },
    components: {
        'card-svg': CardSVG
    },
    methods: {
        checkPassword() {
            if(this.passwordInput == this.password) {
                this.didEnterPassword = true
                this.$cookie.set('site-password', this.password, 10);
                this.$nextTick(() => {
                    this.initCard()
                })
            }
        },
        initCard() {
            svgPanZoom(this.$refs.svg.$el, {
            panEnabled: true,
                /*
                controlIconsEnabled: false
                , zoomEnabled: true
                , dblClickZoomEnabled: true
                , mouseWheelZoomEnabled: true
                , preventMouseEventsDefault: true
                , zoomScaleSensitivity: 0.2
                , minZoom: 0.5
                , maxZoom: 10
                , fit: true
                , contain: false
                , center: true
                , refreshRate: 'auto'
                , beforeZoom: function(){}
                , onZoom: function(){}
                , beforePan: function(){}
                , onPan: function(){}
                , onUpdatedCTM: function(){}
                , customEventsHandler: {}
                , eventsListenerElement: null*/
            });

            var el = $(this.$refs.svg.$el).hide();
            el.delay(1000).fadeIn(3000)

            var newYearEl = $('#happy_new_year');

            newYearEl.hide();
            newYearEl.delay(3000).fadeIn(3000)
        }
    },
    computed: {
    },
    watch: {
    },
    created() {
    },
    mounted() {
        var ps = this.$cookie.get('site-password');
        if(ps == this.password) {
            this.didEnterPassword = true;
            this.$nextTick(() => {
                this.initCard()
            })
        }
    }
}
</script>

<style lang="scss">
* { margin:0; padding:0; }
html, body, #app, .holiday-card { 
    width:100%; height:100%; 
    font-family: 'Slabo 27px', serif;
}
input {
    padding: 30px;
    width: 300px;
    border-radius: 5px;
    box-shadow: none;
    border: none;
    background-color: #eee;
    font-size: 22px;
    text-align: center;
    &:focus {
        outline: none;
    }
}
.holiday-card {
    display: flex;
    align-items: center;
    justify-content: center;
}
#buddy-bear {
}
svg {
    width: 100%;
    height: 100%;
}
</style>