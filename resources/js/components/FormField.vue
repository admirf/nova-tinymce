<template>
    <default-field :field="field" :full-width-content="true">
        <template slot="field">
            <LanguageTabs v-model="currentLocale" :locales="field.locales"/>
            <br />
            <editor :api-key="tinymce_api_key"
                    :id="field.attribute" 
                    v-model="value[currentLocale]"
                    :class="errorClasses"
                    :placeholder="field.name"
                    :init="options"
            ></editor>

            <p v-if="hasError" class="my-2 text-danger">
                {{ firstError }}
            </p>
        </template>
    </default-field>
</template>

<script>
import { FormField, HandlesValidationErrors } from 'laravel-nova'
import Editor from '@tinymce/tinymce-vue'
import LanguageTabs from './LanguageTabs'

export default {
    components: {
        Editor,
        LanguageTabs
    },

    mixins: [FormField, HandlesValidationErrors],

    props: ['resourceName', 'resourceId', 'field'],

    data () {
        return {
            locales: Object.keys(this.field.locales),
            currentLocale: null
        }
    },

    mounted () {
        this.currentLocale = this.locales[0] || null
    },

    computed: {
        tinymce_api_key() {
            return Nova.config.tinymce_api_key
        },
        
        options() {
            let options = this.field.options

            if (options.use_lfm) {
                options['file_browser_callback'] = this.filemanager
            }

            return options
        }
    },

    methods: {
        /*
         * Set the initial, internal value for the field.
         */
        setInitialValue() {
            this.value = this.field.value || {}
        },

        /**
         * Fill the given FormData object with the field's internal value.
         */
        fill(formData) {
            Object.keys(this.value).forEach(locale => {
                formData.append(this.field.attribute + '[' + locale + ']', this.value[locale] || '')
            })
        },

        /**
         * Update the field's internal value.
         */
        handleChange(value) {
            this.value[this.currentLocale] = value
        },

        filemanager(field_name, url, type, win) {
            let x = window.innerWidth || document.documentElement.clientWidth || document.getElementsByTagName('body')[0].clientWidth;
            let y = window.innerHeight|| document.documentElement.clientHeight|| document.getElementsByTagName('body')[0].clientHeight;

            let cmsURL = this.options.path_absolute + this.options.lfm_url + '?field_name=' + field_name;
            if (type == 'image') {
                cmsURL = cmsURL + '&type=Images';
            } else {
                cmsURL = cmsURL + '&type=Files';
            }

            tinyMCE.activeEditor.windowManager.open({
                file : cmsURL,
                title : 'Filemanager',
                width : x * 0.8,
                height : y * 0.8,
                resizable : 'yes',
                close_previous : 'no'
            });
        }

    }
}
</script>
