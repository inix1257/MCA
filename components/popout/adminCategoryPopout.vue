<template>
    <div class="adminPopout">
        <div class="adminPopout__section">
            name
            <input 
                v-model="name"
                class="adminPopout__input"
            >
        </div>
        <div class="adminPopout__section">
            description
            <input 
                v-model="desc"
                class="adminPopout__input"
            >
        </div>
        <div class="adminPopout__section"> 
            <div>
                type
                <select v-model="type">
                    <option :value="'users'">
                        users
                    </option>
                    <option :value="'beatmapsets'">
                        beatmaps
                    </option>
                </select>
            </div>
        </div>
        <div class="adminPopout__section">
            max nominations
            <input 
                v-model.number="nomCount"
                class="adminPopout__input"
            >
        </div>
        <div class="adminPopout__section">
            required
            <select v-model="required">
                <option :value="true">
                    yes
                </option>
                <option :value="false">
                    no
                </option>
            </select>
        </div>
        <selectButton 
            :option="'save'"
            @emit="send"
        />
        <selectButton 
            :option="'cancel'"
            @emit="$emit('cancel')"
        />
    </div>
</template>

<script lang="ts">
import Vue from "vue";
import _ from "lodash";

import button from "../button.vue";

export default Vue.extend({
    components: {
        selectButton: button,
    },
    props: {
        info: {
            type: Object,
            default: function () {
                return {};
            },
        },
    },
    data () {
        return {
            init: {},
            name: "",
            desc: "",
            type: "beatmapsets",
            nomCount: 3,
            required: false,
        };
    },
    updated () {
        if (
            !_.isEqual(this.info, this.init) &&
            this.info.name && 
            this.info.desc && 
            this.info.type && 
            this.info.nomCount && 
            this.info.required
        ) {
            this.name = this.info.name;
            this.desc = this.info.desc;
            this.type = this.info.type;
            this.nomCount = this.info.nomCount;
            this.required = this.info.required;
        }
    },
    methods: {
        send() {
            this.$emit("send", {
                name: this.name,
                desc: this.desc,
                type: this.type,
                nomCount: this.nomCount,
                required: this.required,
            });
        },
    },
});
</script>

<style lang="scss">
.adminPopout {
    position: fixed;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    background-color: rgba(0,0,0,0.9);
    box-shadow: 0 0 4px rgba(0,0,0,0.9);
}

.adminPopout__section {
    display: flex;
    flex-direction: column;

    padding: 15px;
}

.adminPopout__input {
    font-family: 'Red Hat Display', sans-serif;

    color: black;

    background-color: rgb(115,115,115);
    box-shadow: 0 0 8px rgba(115, 115, 115, 0.61);

    border: 0;

    &:focus {
        outline: none;
    }
}
</style>