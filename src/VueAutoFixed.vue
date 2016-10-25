<template>
    <div v-el:box :style="style" :class="class" style="background: #ddd">
        <div v-el:dom :style="{transition:'all '+time+'s','-webkit-transition':'all '+time+'s'}">
            <slot></slot>
        </div>

    </div>
</template>


/*!
* vue-auto-animation v0.0.1 (https://github.com/johnnyGoo/vue-auto-animation)
* Author: Johnny chen
*
* Copyright 2013-2015 Johnny chen
*/
<script>

    var count = 0;
    if (!window.Smart) {
        throw 'vue-auto-animation required smart.js'
    }
    var Smart = window.Smart;
    var Css = Smart.Css;
    var Event = Smart.Event;
    var Utils = Smart.Utils;
    var _ = Smart._;


    // 注册
    export default {

        // 声明 props
        props: {
            style: {
                type: Object,
                default: function () {
                    return {}
                }
            },
            class: {
                type: Object,
                default: function () {
                    return {}
                }
            },
            normal: {
                type: Object,
                default: function () {
                    return {}
                }
            },
            fixed: {
                type: Object,
                default: function () {
                    return {}
                }
            }, time: {
                type: Number,
                default: 0.5
            }, always: {
                type: Boolean,
                default: false
            }
        },
        data: function () {
            count++;
            return {
                size: {width: 0, height: 0}, windowSize: {}
            }
        },
        methods: {
            _update: function () {
                if (this.always) {
                    return Css.smartCss(this.$els.dom, this.fixed);
                }
                var size = this.$els.box.getBoundingClientRect();
                var show = (size.top < 0 || size.bottom > this.windowSize.height) || (size.left < 0 || size.right > this.windowSize.width)
                if (show) {
                    Css.smartCss(this.$els.dom, this.fixed);
                } else {
                    Css.smartCss(this.$els.dom, this.normal);
                }
            }
        },
        computed: {},
        ready: function () {
            var me = this;
            // this._update();
//           setInterval(this._update, 100);
            var size = this.$els.box.getBoundingClientRect();
            // this.size.width = size.width;
            this.size.height = size.height;
            Css.smartCss(this.$els.box, {height: size.height});


            function onResize() {
                me.windowSize = Utils.windowSize();  me._update();
            }

            var rm_widow = Event.windowEvent('resize', function () {
                onResize();
                me._update();
                // rm_widow();
            })
            var rm_scoll = Event.bindEvent(window, 'scroll', function () {
                me._update();
                //   console.log('scroll')
            });
            onResize()

        }
    }


</script>