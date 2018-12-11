<template>
    <panel-item :field="field">
        <template slot="value">
            <LanguageTabs v-model="locale" :locales="field.locales" />
            <br />
            <excerpt :content="field.value[locale]" :should-show="field.shouldShow" />
        </template>
    </panel-item>
</template>

<script>
    import LanguageTabs from './LanguageTabs'

    export default {
        components: {LanguageTabs},
        props: ['resource', 'resourceName', 'resourceId', 'field'],

        data () {
            return {
                locales: Object.keys(this.field.locales),
                locale: null
            }
        },

        mounted () {
            this.locale = this.locales[0] || null
        }
    }
</script>
