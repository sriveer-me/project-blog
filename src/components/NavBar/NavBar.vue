<template>
    <nav id="NavBar">
        <router-link to="/" class="brandLogo" id="brandLogo"
         v-show="showBrandLogo"
        ><img :src="require('../../assets/img/logo.svg')" alt="brand-logo"></router-link>

        <NavSearchBar
         @navSearchBar:maximize_started="navSearchBarMaximize_started" @navSearchBar:maximize_completed="navSearchBarMaximize_completed"
         @navSearchBar:minimize_started="navSearchBarMinimize_started" @navSearchBar:minimize_completed="navSearchBarMinimize_completed"

         @searchBar:update="searchBarUpdated"
        />       
    </nav>
</template>

<script>
import NavSearchBar from './NavSearchBar.vue';

import {isMobile} from '@/javascript/breakpoints';
import {onScroll} from '@/javascript/scrollCore.js';

export default {
    name: "NavBar",
    components:{
        NavSearchBar
    },
    data(){
        return{
            showBrandLogo: true,
            minimized: false
        }
    },
    methods:{
        navSearchBarMinimize_started(){
            if(isMobile()){
                this.showBrandLogo = true;
                console.log('minimize start')
            }
        },
        navSearchBarMinimize_completed(){
            if(isMobile()){
                let nav = this.$el;
                nav.style.justifyContent = "space-between";

                console.log('minimize completed')
            }
        },

        navSearchBarMaximize_started(){
            if(isMobile()){
                let nav = this.$el;
                nav.style.justifyContent = "flex-end";

                console.log('maximize start')
            }
        },
        navSearchBarMaximize_completed(){
            if(isMobile()){
                this.showBrandLogo = false;
                console.log('maximize completed')
            }
        },

        searchBarUpdated(payload){
            this.$emit('searchBar:update',payload);
        }

    },
    mounted(){
        onScroll(function(){
            let scrolledY = window.pageYOffset;
            if(scrolledY > 0 && this.minimized === false){
                this.$el.classList.add('minimize');
                this.minimized = true;
            } else if(scrolledY === 0){
                this.$el.classList.remove('minimize');
                this.minimized = false;
            }
        }.bind(this))

        //for bug fixing purpose as if window size changes without the input loosing focus.. then space-between is never set until refresh
        window.addEventListener('resize',function(){
            if(!isMobile()){
                let nav = this.$el;
                nav.style.justifyContent = "space-between";
            }
        }.bind(this))
    }
}
</script>

<style lang="scss" scoped>
    @use '../../assets/scss/setting' as *;

    nav{
        transition: min-height 0.3s ease-in-out;
        overflow-y: hidden;

        display: flex;
        position: sticky;
        z-index: 1000;
        top: 0;

        justify-content: space-between;
        align-items: center;

        padding: 0 var(--spacing-large);

        min-height: 100px;
        @include for-tablet-portrait-up{
            min-height: 150px;
        }
        &.minimize{
            min-height: 75px;
        }

        background: map-get($light,"light");
    }

    .brandLogo{
        position: relative;
        z-index: 1;

        font-family: Permanent Marker, cursive;
        font-style: normal;
        font-weight: normal;
        font-size: var(--font-larger);
        line-height: 58px;

        color: map-get($dark,"dark");
    }
</style>