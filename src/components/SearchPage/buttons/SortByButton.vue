<template>
    <div class="sortByButton">
        <Button buttonName="Sort By" @button:click="handleButtonClick">
            <template v-slot:iconSlot>
                 <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 16 16"><g fill="none"><path d="M10.73 13.79c.29.28.75.28 1.04 0l2.75-2.65a.75.75 0 1 0-1.04-1.08L12 11.486V2.75a.75.75 0 0 0-1.5 0v8.736L9.02 10.06a.75.75 0 1 0-1.04 1.08l2.75 2.65zM5.28 2.22a.75.75 0 0 0-1.06 0L1.47 4.97a.75.75 0 0 0 1.06 1.06L4 4.56v8.69a.75.75 0 0 0 1.5 0V4.56l1.47 1.47a.75.75 0 0 0 1.06-1.06L5.28 2.22z" fill="currentColor"></path></g></svg>
            </template>
        </Button>
        <Modal
            cardTitle="Sort By -"
            ref="modal"
        >
            <template #content>
                <!-- not moblie radio button -->
                <n-radio-group v-model:value="value" name="selectSortBy" v-show="!isMobile">
                    <n-radio-button value="ascending">
                        Ascending Order Alphabetically By Title
                    </n-radio-button>
                    <n-radio-button value="descending">
                        Descending Order Alphabetically By Title
                    </n-radio-button>
                </n-radio-group>

                <!-- the radio button on mobile -->
                <n-radio-group v-model:value="value" name="selectSortBy" v-show="isMobile">
                    <n-radio-button value="ascending">
                        Ascending Order
                    </n-radio-button>
                </n-radio-group>
                <n-radio-group v-model:value="value" name="selectSortBy" v-show="isMobile">
                    <n-radio-button value="descending">
                        Descending Order
                    </n-radio-button>
                </n-radio-group>

            </template>
        </Modal>
    </div>
</template>

<script>
import {NRadioGroup,NRadioButton} from 'naive-ui';
import Button from '@/components/Wrappers/Button.vue';
import Modal from "@/components/Wrappers/Modal.vue";

import {isMobile} from '@/javascript/breakpoints.js';

export default {
    name: "SelectTagsButton",
    components:{
        Button,
        Modal,
        NRadioGroup,
        NRadioButton
    },
    data(){
        return{
            value: "ascending",
            emitEvents: false
        }
    },
    watch: {
        value:{
            handler(newValue){
                if(this.emitEvents === false) //donot emit events if not required
                    return;

                let emitValue = false;
                if(newValue === "ascending")
                    emitValue = true;
                else emitValue = false;
                
                this.$emit('tags:changed', emitValue);
            },
            deep: true
        }
    },
    methods:{
        handleButtonClick(){
            this.$refs.modal.show();
        },

        isMobile(){
            return isMobile();
        }
    },
    created(){
        this.emitEvents = true;
    }
}
</script>

<style lang="scss" scoped>
    @use '@/assets/scss/setting' as *;

    .sortByButton{
        flex: 1 1 100%;
        display: flex;
        align-items: center;

        .n-button{
            flex: 1 1 100%;
        }
    }

    .n-radio-group{
        width: 100%;
        text-align: center;

        .n-radio-button{
            width: 100%;
        }
    }
    @include for-tablet-portrait-up{
        .n-radio-group{
        width: auto;
        text-align: left;

        .n-radio-button{
            width: auto;
        }
    }
    }
    
</style>