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
import Hammer from 'hammerjs'
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

            // Don't use window.onLoad like this in production, because it can only listen to one function.
        var eventsHandler = {
            haltEventListeners: ['touchstart', 'touchend', 'touchmove', 'touchleave', 'touchcancel'], 
            init: function(options) {
                var instance = options.instance;
                var initialScale = 1
                var pannedX = 0
                var pannedY = 0
                // Init Hammer
                // Listen only for pointer and touch events
                this.hammer = Hammer(options.svgElement, {
                    inputClass: Hammer.SUPPORT_POINTER_EVENTS ? Hammer.PointerEventInput : Hammer.TouchInput
                })
                
                // Enable pinch
                this.hammer.get('pinch').set({enable: true})
            
                // Handle double tap
                this.hammer.on('doubletap', function(ev){
                    instance.zoomIn()
                })
                
                // Handle pan
                this.hammer.on('panstart panmove', function(ev){
                    // On pan start reset panned variables
                    if (ev.type === 'panstart') {
                        pannedX = 0
                        pannedY = 0
                    }
                    // Pan only the difference
                    instance.panBy({x: ev.deltaX - pannedX, y: ev.deltaY - pannedY})
                    pannedX = ev.deltaX
                    pannedY = ev.deltaY
                })
                
                // Handle pinch
                this.hammer.on('pinchstart pinchmove', function(ev){
                    // On pinch start remember initial zoom
                    if (ev.type === 'pinchstart') {
                        initialScale = instance.getZoom()
                        instance.zoom(initialScale * ev.scale)
                    }
                    instance.zoom(initialScale * ev.scale)
                })
                
                // Prevent moving the page on some devices when panning over SVG
                options.svgElement.addEventListener('touchmove', function(e){ e.preventDefault(); });
            }, 
            destroy: function() {
                this.hammer.destroy()
            }
        }

        // Expose to window namespace for testing purposes
        svgPanZoom(this.$refs.svg.$el, {
            zoomEnabled: true, 
            controlIconsEnabled: true, 
            fit: 1, 
            center: 1, 
            customEventsHandler: eventsHandler
        });

            /*svgPanZoom(this.$refs.svg.$el, {
                panEnabled: true,
            });*/

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