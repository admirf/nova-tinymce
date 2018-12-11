<template>
    <div v-if="value">
        <ul>
            <li class="inline-block font-bold cursor-pointer mr-2 animate-text-color select-none border-primary"
                v-for="(lang, key) in locales" @click="changeLocale(key)"
                :class="{ 'text-60': key !== value, 'text-primary border-b-2': key === value }"
            ><strong>{{ lang }}</strong></li>
        </ul>
    </div>
</template>

<script>
    export default {
        name: "LanguageTabs",
        props: ['value', 'locales'],
        methods: {
            changeLocale (key) {
                this.$emit('input', key)
            },
            getActive (key) {
                if (key === this.value) {
                    return 'active'
                }
            }
        }
    }
</script>

<style scoped>
    .active {
        text-decoration: underline;
        color:
    }

    ul {
        padding-left: 0;
    }

    .item {
        display: inline;
        cursor: pointer;
        margin-right: 8px;
    }
</style>