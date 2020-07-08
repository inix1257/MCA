<template>
    <div
        :style="loadingTransition"
        class="main"
    >
        <headerComponent
            :mode="mode"
            :user="user"
            :dropdown="dropdown"
            @dropdown="dropdown=!dropdown"
        />
        <dropdownComponent
            v-if="dropdown"
            :user="user"
            @click="dropdown=false"
        />
        <transition name="fade">
            <nuxt
                :user="user" 
                :eligible="eligible"
                :mode="mode"
                @mode="updateMode"
            />
        </transition>
        <footerComponent />
    </div>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";

import header from "../components/header/index.vue";
import dropdown from "../components/dropdown.vue";
import footer from "../components/footer/index.vue";

import { UserMCAInfo } from "../../CorsaceModels/user";

export default Vue.extend({
    components: {
        "headerComponent": header,
        "dropdownComponent": dropdown,
        "footerComponent": footer,
    },
    data () {
        return {
            loaded: false,
            user: {} as UserMCAInfo,
            dropdown: false,
            eligible: false,
            mode: "standard",
        };
    },
    computed: {
        loadingTransition: function () {
            if (!this.loaded) {
                return {
                    opacity: 0,
                };
            } else {
                return {
                    opacity: 1,
                };
            }
        },
    },
    mounted: async function () {
        await this.update();

        const localMode = localStorage.getItem("mode");
        if (localMode && /^(standard|taiko|fruits|mania|storyboard)$/.test(localMode))
            this.mode = localMode;

        this.loaded = true;
    },
    methods: {
        update: async function() {
            try {
                const data = (await axios.get(`/api/user`)).data;

                if (!data.error) {
                    this.user = data.user;
                    for (const eligibility of this.user.eligibility) {
                        if (eligibility.year === (new Date).getUTCFullYear()) {
                            this.eligible = true;
                        }
                    }
                }
            } catch (err) {
                console.error(err);
            }
        },
        updateMode (val) {
            if (/^(standard|taiko|fruits|mania|storyboard)$/.test(val)) {
                this.mode = val;
                localStorage.setItem("mode", this.mode);
            }
        },
    },
});
</script>

<style lang="scss">
.main {
    transition: opacity 0.5s ease-out;
}
.fade-enter-active, .fade-leave-active {
    transition: opacity .25s ease-out;
}
.fade-enter, .fade-leave-to {
    opacity: 0;
}
</style>