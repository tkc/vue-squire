<template>
    <div id="app">

        <div class="row header">
            <div class="col-sm-12 col-sm-offset-0 col-md-8 col-md-offset-2">
                <h1 class="">Squire SandBox</h1>
                <h2>Using Vue.js</h2>
            </div>
        </div>

        <div class="row">
            <div class="col-sm-12 col-sm-offset-0 col-md-8 col-md-offset-2">
                <div class="menu">
                    <div class="group">
                        <div @click="changeH1" class="item" :class="{ active: isH1}">
                            <i class="fa fa-header"></i>
                        </div>
                        <div @click="changeBold" class="item" :class="{ active: isBold}">
                            <i class="fa fa-bold"></i>
                        </div>
                        <div @click="changeItalic" class="item" :class="{ active: isItalic}">
                            <i class="fa fa-italic"></i>
                        </div>
                    </div>
                    <div class="group">
                        <div @click="insertLink" id="makeLink" class="item drop-target">
                            <i class="fa fa-link"></i>
                        </div>
                        <div @click="runImage" id="insertImage" class="item drop-target">
                            <i class="fa fa-picture-o"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-sm-5 col-sm-offset-5 col-md-8 col-md-offset-2">
                <div id="squire-editor"></div>
            </div>
        </div>

        <link_modal :show="imageModalShow" v-on:callback="insertLink"></link_modal>
        <upload :action="runUpload" v-on:callback="insertImage"></upload>

    </div>
</template>

<script>

    const Squire = require('squire-rte');
    import LinkModal from './components/LinkModal.vue';
    import Upload from './components/Upload.vue';

    export default {
        components: {
            "link_modal": LinkModal,
            "upload": Upload,
        },
        props: [
            'defaultHtml'
        ],
        data () {
            return {
                outer: "",
                editor: {},
                isH1: false,
                isBold: false,
                isItalic: false,
                imageModalShow: false,
                runUpload: false,
            };
        },
        watch: {
            outer(val){
                // console.log(val);
            },
            isH1(val) {
                const tag = val ? 'h2' : 'p';
                this.editor.setConfig({
                    blockTag: tag,
                    blockAttributes: {'class': ''}
                });
            },
            isBold(val) {
                val ? this.editor['bold'](null) : this.editor['removeBold'](null);
            },
            isItalic(val) {
                val ? this.editor['italic'](null) : this.editor['removeItalic'](null);
            },
        },
        methods: {
            changeH1(){
                this.isH1 = !this.isH1;
                this.isBold = false;
                this.isItalic = false;
            },
            changeBold(){
                this.isH1 = false;
                this.isBold = !this.isBold;
                this.isItalic = false;
            },
            changeItalic(){
                this.isH1 = false;
                this.isBold = false;
                this.isItalic = !this.isItalic;
            },
            insertLink(val){
                if (!val.name || !val.url) {
                    this.imageModalShow = !this.imageModalShow;
                } else {
                    this.imageModalShow = !this.imageModalShow;
                    const html = "<a target='_blank' href='" + val.name + "'>" + val.url + "</a>";
                    this.editor.insertHTML(html);
                }
            },
            runImage(){
                this.runUpload = !this.runUpload;
                // TODO :
                setTimeout(() => {
                    this.runUpload = !this.runUpload;
                }, 50);
            },
            insertImage(val){
                const html = "<img class='editor_image' src='" + val.src + "'/>";
                this.editor.insertHTML(html);
            },
        },
        mounted() {

            const self = this;

            this.editor = new Squire(document.getElementById('squire-editor'), {
                blockTag: 'p',
                blockAttributes: {'class': ''},
            });

            this.editor.addEventListener("keyup", () => {
                self.outer = self.editor.getHTML()
            });

            const dummyHtml = "<p></p><h2>Title1</h2><p></p><h2>Title2</h2><p></p><h2>Title3</h2>";
            !this.defaultHtml ? this.editor.setHTML(dummyHtml) : "";
        }
    }
</script>

<style>

    .header {
        padding: 50px 0 30px;
        color: #fff;
        text-align: center;
        background: #1d193d;
        margin-bottom: 20px;
    }

    .header h2 {
        margin-bottom: 1em;
        font-size: 3em;
        font-weight: 300;
        text-transform: lowercase;
        color: #afaedf;
    }

    .menu .item {
        color: #000;
        float: left;
        background: #FFF;
        padding: 10px;
        border-left: 1px #EEE solid;
        border-bottom: 3px transparent solid;
        -webkit-font-smoothing: subpixel-antialiased
    }

    .menu .group {
        border-radius: 3px;
        display: inline-block;
        border: 1px #EEE solid;
        margin: 5px
    }

    .menu .group .item .flip {
        -ms-transform: rotateY(180deg);
        -webkit-transform: rotateY(180deg);
        -moz-transform: rotateY(180deg);
        transform: rotateY(180deg)
    }

    .menu .group .item:hover, .menu .item:first-child:hover {
        border-bottom: 3px #55ACEE solid;
    }

    .menu .item:first-child {
        border-left: none;
    }

    .menu {
        text-align: center;
        -webkit-touch-callout: none;
        -webkit-user-select: none;
        -khtml-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
    }

    #squire-editor {
        -moz-box-sizing: border-box;
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
        min-height: 200px;
        border: 1px solid #888;
        padding: 1em;
        background: transparent;
        color: #2b2b2b;
        font: 13px/1.35 Helvetica, arial, sans-serif;
        cursor: text;
        width: 100%;
        background: repeat;
    }

    .active {
        background: #1d193d !important;
        color: aliceblue !important;
    }

    .editor_image {
        width: 100%;
    }
</style>
