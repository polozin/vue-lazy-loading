<template>
    <div class="master">
        <div  class="header">
            <button v-show="!started" @click="start">Show menu</button>
        </div>
        <div class="content" @scroll.passive="handleScroll"  >
        <div v-for="(item, index) in list" class="item" :id="'i' + index " :key="index">
            {{item}}
        </div>
        </div>
        <div class="footer">
            <div class="author">@Constantin Polozin 2021</div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "VirtualList",
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
           // window.addEventListener('scroll', this.handleScroll);
            this.payload = JSON.parse(JSON.stringify(this.items));
        },
        destroyed () {
         //   window.removeEventListener('scroll', this.handleScroll);
        },
        methods: {
            handleScroll(){
               let lastItem = document.getElementById('i' + (this.list.length -1));
               let position = lastItem.getBoundingClientRect().top - window.innerHeight;
               if( position < - 50 ){
                   this.addItems();
               }
               console.log(lastItem.getBoundingClientRect().top - window.innerHeight);
            },
            start(){
              if(! this.started){
                  this.started = true;
                  this.addItems();
              }
            },
            addItems() {
                if (this.payload.length) {
                    this.list.push( ...this.payload.splice(0, this.$options.PAGE_COUNT));
                    this.list = JSON.parse(JSON.stringify(this.list));
                }
            }
        }
    };
</script>

<style scoped>
    .master {
        height: 100%;
        min-height: 100%;
        display: flex;
        flex-direction: column;
    }
    .content{
        height: 80vh;
        overflow-y: auto;
    }
    .header{
        height: 30px;
        padding: 20px;
    background-color: brown;}
    .footer{
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
    .item{
        border-bottom: 1px solid #777777;
    padding: 10px 0 10px 20px;
    font-size: 20px;
    margin-bottom: 2px;}

</style>
