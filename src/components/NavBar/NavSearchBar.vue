<template>
    <div class="main">
        <div id="searchIcon" class="icon-holder" 
             @mousedown="searchIconClicked"
        >
            <span class="iconify icon" data-icon="bx:bx-search-alt-2"></span>
        </div>
        <input type="text" id="NavSearchBar" class="subtitle mobile-hidden" placeholder="Visual-Novel"
               @focus="searchFieldFocused" @blur="searchFieldLostFocus" v-model="inputText"
        >
        <div class="border mobile-hidden"></div>
        <div class="borderCover mobile-hidden"></div>
    </div>
</template>

<script>
export default {
    name: "ResponsiveNavSearchBar",
    data(){
        return{
            mobileHiddenRemoved: false,
            inputText: ''
        }
    },
    watch:{
        inputText(newValue){
            this.$emit('searchBar:update',newValue);
        }
    },
    methods:{
        search(){
            console.log('search performed on the nav search bar');
        },
        searchIconClicked(e){
            e.preventDefault();

            if(this.mobileHiddenRemoved === false)
                this.removeMobileHidden();
            else this.search();
        },


        activateMobileHidden(){
            let input = this.$el.querySelector('input');
            input.classList.add('mobile-hidden');

            let border = this.$el.querySelector('.border');
            border.classList.add('mobile-hidden');

            let borderCover = this.$el.querySelector('.borderCover');
            borderCover.classList.add('mobile-hidden');

            this.mobileHiddenRemoved = false;
        },
        removeMobileHidden(){
            let input = this.$el.querySelector('input');
            input.classList.remove('mobile-hidden');

            let border = this.$el.querySelector('.border');
            border.classList.remove('mobile-hidden');

            let borderCover = this.$el.querySelector('.borderCover');
            borderCover.classList.remove('mobile-hidden');

            input.focus();
            this.mobileHiddenRemoved = true;
            this.$emit('navSearchBar:maximize_started');
        },


        searchFieldFocused(){
            let borderCover = this.$el.querySelector('.borderCover');
            borderCover.classList.add('removeCover');

            let navSearchBar = this.$el;
            navSearchBar.classList.add('expand');

            this.removeMobileHidden();

            this.$router.push('search')
        },
        searchFieldLostFocus(){
            let borderCover = this.$el.querySelector('.borderCover');
            borderCover.classList.remove('removeCover');

            let navSearchBar = this.$el;
            navSearchBar.classList.remove('expand');

            this.activateMobileHidden();
            this.$emit('navSearchBar:minimize_started');
        }
    },
    mounted(){
        let input = this.$el.querySelector('input');
            input.addEventListener('transitionend',function(e){
                if(e.propertyName === 'width'){
                    let input = e.target;
                    let inputStyle = window.getComputedStyle(input);
                    let width = parseInt(inputStyle.width);
                    
                    if(width === 0){
                        this.$emit('navSearchBar:minimize_completed');
                    }
                    else this.$emit('navSearchBar:maximize_completed'); 
                }
            }.bind(this));
    }
}
</script>

<style lang="scss" scoped>
    @use '../../assets/scss/setting' as *;

    .main{
        transition: 1s ease-in-out flex;

        display: flex;
        position: relative;
        z-index: 3;
        overflow-x: hidden;

        flex: 1 1 50%;
        &.expand{
            flex: 0 0 100%;
        }
        @include for-tablet-portrait-up{
            flex: 0 0 50%;
            &.expand{
                flex: 0 0 70%;
            }
        }
        

        min-height: 58px;

        .icon-holder{
            position: absolute;
            right: 0;
            top : 20%;

            svg{
                width : 35px;
                height: 35px;
                
                color: $accent;
                cursor: pointer;
            }

             z-index: 4;
        }

        input{
            transition: 1s width ease-in-out, 
                        1s padding ease-in-out;

            border: none;
            outline: none;

            width: calc(100% - 2px);
            padding: 10px;
            padding-right: 51px;

            margin-left: auto;

            //all these so that the border can remain visible
            margin-right: 1px;
            margin-top: 1px;
            margin-bottom: 1px;

            &.mobile-hidden{
                width: 0;
                padding-left: 0;
                padding-right: 0;
            }
            @include for-tablet-portrait-up{
                &.mobile-hidden{
                    width: calc(100% - 2px);

                    padding-right: 51px;
                    padding-left: 10px;
                }
            }

            z-index: 3;
        }
    }

    .main{
        .border{
            transition: 1s width ease-in-out;

            position: absolute;
            border: 1px solid $accent;

            top: 0;
            right: 0;
            width: 100%;
            height: 100%;

             &.mobile-hidden{
                width: 0%;
                height: 0%;
            }
            @include for-tablet-portrait-up{
                &.mobile-hidden{
                    width: 100%;
                    height: 100%;
                }
            }

            z-index: 1;
        }
        .borderCover{
            transition: 1s ease-in-out transform,
                        1s ease-in-out width;

            position: absolute;
            border: 1px solid white;

            top: 0;
            right: 0;
            width: 100%;

            &.mobile-hidden{
                width: 0%;
                height: 0%;
            }
            @include for-tablet-portrait-up{
                &.mobile-hidden{
                    width: 100%;
                    height: 100%;
                }
            }

            height: 100%;
            z-index: 2;

            background-color: white;

            &.removeCover{
                transform: translateX(110%);
            }
        }
    }
</style>