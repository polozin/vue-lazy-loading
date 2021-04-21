<template>
    <div class="master">
        <div class="header">
            <button  @click="start">Show Items</button>
        </div>
        <div class="content" @scroll.passive="handleScroll">
            <transition-group name="list" tag="p">
            <div v-for="(item, index) in list" class="item" :id="'i' + index " :key="index">
                {{item}}
            </div>
            </transition-group>
        </div>
        <div class="footer">
            <div class="author">@Constantin Polozin 2021</div>
        </div>
    </div>
</template>

<script>
    //project in github https://github.com/polozin/vue-lazy-loading
    export default {
        name: "LazyLoading",
        PAGE_COUNT: 20,
        props: {
            items: Array
        },
        data: () => {
            return {
                list: [],
                payload: [],
                started: false,
            };
        },
        created() {
            this.payload = JSON.parse(JSON.stringify(this.items));
        },
        methods: {
            handleScroll() {
                let lastItem = document.getElementById('i' + (this.list.length - 1));
                let position = lastItem.getBoundingClientRect().top - window.innerHeight;
                if (position < -50) {
                    this.addItems();
                }
            },
            start() {
                if (!this.started) {
                    this.started = true;
                    this.addItems();
                }
            },
            addItems() {
                if (this.payload.length) {
                    this.list.push(...this.payload.splice(0, this.$options.PAGE_COUNT));
                }
            }
        }
    };
</script>

<style scoped>
    .master {
        height: 100%;
        min-height: 100%;

    }

    .content {
        height: 80vh;
        overflow-y: auto;
    }

    .header {
        height: 22px;
        padding: 20px;
        background-color: brown;
    }

    .footer {
        position: fixed;
        bottom: 0;
        width: 100%;
        border-top: solid 3px #777777;
        background-color: #ffffff;
    }

    .author {
        text-align: center;
        padding: 20px 0;
        font-size: 20px;
    }

    .item {
        border-bottom: 1px solid #777777;
        padding: 10px 0 10px 20px;
        font-size: 20px;
        margin-bottom: 2px;
    }
    .list-enter-active, .list-leave-active {
        transition: all 2s;
    }
    .list-enter, .list-leave-to /* .list-leave-active до версии 2.1.8 */ {
        opacity: 0;
        color: brown;
        font-size: 19px;
    }
</style>
