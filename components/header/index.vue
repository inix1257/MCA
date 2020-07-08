<template>
    <div class="header">
        <img src="../../../CorsaceAssets/img/ayim-mca/site/2019.png">
        <nuxt-link
            class="header__title header__title--active"
            :to="'/'"
            v-html="$t('mca_ayim.header.mca')"
        />
        <a class="header__seperator"><img src="../../../CorsaceAssets/img/ayim-mca/site/l.png"></a>
        <a
            class="header__title"
            href="ayim.html"
            v-html="$t('mca_ayim.header.ayim')"
        />
        <div 
            v-if="user && user.osu"
            class="header__login"
        >
            <div class="header__login--text header__login--flex">
                <div class="header__login--gray">
                    {{ $t('mca_ayim.header.welcomeBack') }}
                </div>
                <div>
                    {{ user.osu.username }}
                </div>
            </div>
            <div
                class="header__login--profile header__login--flex" 
                @click="toggleDropdown"
            >
                <img 
                    :src="avatarURL"
                    class="header__login--image"
                >
                <div 
                    :class="triangleClass"
                    class="triangle" 
                />
            </div>
        </div>
        <a
            v-else
            class="header__login"
            @click="login=!login; $router.push({ path: $route.path, query: { login: true }})"
        >
            {{ $t('mca_ayim.header.login') }}
        </a>

        <login 
            v-if="login"
            :user="user"
            @close="login=false; $router.push({ path: $route.path })"
        />
    </div>
</template>

<script lang="ts">
import Vue, { PropOptions } from "vue";

import login from "./login.vue";

import { UserMCAInfo } from "../../../CorsaceModels/user";

export default Vue.extend({
    components: {
        "login": login,
    },
    props: {
        user: {
            type: Object,
            required: true,
        } as PropOptions<UserMCAInfo>,
        dropdown: Boolean,
        mode: {
            type: String,
            default: "standard",
        },
    },
    data () {
        return {
            login: false,
            modes: ["standard", "taiko", "fruits", "mania", "storyboard"],
        };
    },
    computed:  {
        avatarURL (): string  {
            return this.user && this.user.osu ? this.user.osu.avatar + "?" + Math.round(Math.random()*1000000) : "";
        },
        triangleClass (): Record<string, any>  {
            const className = `triangleActive--${this.mode}`;
            const obj = {};
            obj[className] = this.dropdown;
            return obj;
        },
    },
    mounted: function () {
        if (this.$route.query.login && (!this.user?.osu?.userID || !this.user?.discord?.userID))
            this.login = true;
    },
    methods: {
        toggleDropdown() {
            this.$emit("dropdown");
        },
    },
});
</script>

<style lang="scss">
.header {
	display: flex;
	background-color: #000;
	border-bottom-style: solid;
	border-bottom-color: #FFF;
	align-items: center;
    width: 100%;
    flex: 0 0 auto;
	padding: 0 2%;
	position: relative;

	a {
		float: left;
		text-decoration: none;
	}
}

.header__seperator {
	margin-left: 20px;
}

.header__title {
	font-size: 1.5rem;
	line-height: 0.86;
	letter-spacing: 10px;
	color: #4c4c4c;
	margin-left: 3%;

	&--active {
		color: #fff;
	}
}

.header__login {
	display: flex;
	text-align: right;
	font-size: 1.25rem;
	line-height: 1.19;
	letter-spacing: 1.89px;
	color: #d8d8d8;
	cursor: pointer;
	align-items: center;
	justify-content: flex-end;
    margin-left: auto;

	&--text {
		cursor: initial;
		padding-right: 10px;
	}

	&--gray {
    white-space: nowrap;
		font-size: 1rem;
		color: #6f6f6f;
	}

	&--flex {
		flex: 1;
	}

	&--profile {
		display: flex;
		align-items: center;
    justify-content: space-evenly;
	}

	&--image {
		border-radius: 50%;

		width: 50%;
    min-width: 45px;

    padding-right: 10px;
	}
}

@media (max-width: 1080px) {
    .header__title {
        font-size: 1rem;
    }

    .header__login {
        font-size: 0.8rem;

        &--gray {
            font-size: 0.7rem;
        }
    }
}
</style>