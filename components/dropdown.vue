<template>
    <transition name="fade">
        <div class="dropdown">
            <nuxt-link :to="'/staff/categories'">
                CATEGORIES
            </nuxt-link>
            <nuxt-link :to="'/staff/requests'">
                REQUESTS
            </nuxt-link>
            <nuxt-link :to="'/api/logout'">
                {{ $t('mca_ayim.header.logout') }}
            </nuxt-link>
        </div>
    </transition>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
    props: {
        user: {
            type: Object,
            default: () => {
                return {};
            },
        },
    }, 
});
</script>

<style lang="scss">
.dropdown {
    letter-spacing: 5px;
	position: absolute;
    cursor: pointer;
	right: 0;
    background-color: #0f0f0f;
    border-radius: 0 0 10px 10px;
    box-shadow: 0px 0px 8px white;
    margin-right: 2%;
    display: flex;
    flex-direction: column;
    z-index: 100;
    width: 15%;
    
    a {
        padding: 5% 0;
        color: white;
        text-decoration: none;
        text-align: center;
        transition: background-color 0.25s, color 0.25s;

        &:hover {
            background-color: white;
            color: #0f0f0f;
        }
    }
}

.fade-enter-active, .fade-leave-active {
    transition: opacity .25s;
}
.fade-enter, .fade-leave-to {
    opacity: 0;
}
</style>