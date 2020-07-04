<template>
    <div v-if="loaded">
        <headerComponent 
            :user="user"
            :dropdown="dropdown"
            @dropdown="dropdown=!dropdown"
        />
        <dropdownComponent
            v-if="dropdown"
            :user="user"
            @click="dropdown=false"
        />
        <nuxt 
            :user="user" 
            :eligible="eligible"
        />
        <footerComponent />
    </div>
</template>

<script>
import axios from "axios";

import header from "../components/header/header";
import dropdown from "../components/dropdown";
import footer from "../components/footer/footer";

export default {
    components: {
        "headerComponent": header,
        "dropdownComponent": dropdown,
        "footerComponent": footer,
    },
    data () {
        return {
            loaded: false,
            user: null,
            dropdown: false,
            eligible: false,
        };
    },
    created: async function () {
        await this.update();
        this.loaded = true;
    },
    methods: {
        update: async function() {
            try {
                const data = (await axios.get(`/api/user`)).data;

                if (!data.error) {
                    this.user = data.user;
                    for (const eligibility of this.user.mcaEligibility) {
                        if (eligibility.year === (new Date).getUTCFullYear) {
                            this.eligible = true;
                        }
                    }
                }
            } catch (err) {
                console.error(err);
            }
        },
    },
};
</script>