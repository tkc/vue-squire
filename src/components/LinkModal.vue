<template>
    <transition name="modal" v-if="show">
        <div class="modal-mask">
            <div class="modal-wrapper">
                <div class="modal-container">

                    <div class="modal-header">
                        <slot name="header">
                            link
                        </slot>
                    </div>

                    <div class="modal-body">
                        <div class="form-group">
                            <label>Name</label>
                            <input type="text" v-model="name" class="form-control" placeholder="Name">
                        </div>
                        <div class="form-group">
                            <label>Url</label>
                            <input type="url" v-model="url" class="form-control" placeholder="URL">
                        </div>
                    </div>

                    <div class="modal-footer">
                        <slot name="footer">
                            <button @click="process" type="submit" class="btn btn-default">Cancel</button>
                            <button @click="process" type="submit" class="btn btn-default">Submit</button>
                        </slot>
                    </div>

                </div>
            </div>
        </div>
    </transition>
</template>

<script>
    export default{
        props: [
            'show'
        ],
        data () {
            return {
                name: "",
                url: "",
            };
        },
        methods: {
            process(){
                const data = {
                    'name': this.name,
                    'url': this.url
                };
                this.$emit('callback', data);
                this.name = "";
                this.url = "";
            }
        },
        mounted(){
        }
    }
</script>

<style>

    input {
        width: 100%;
        margin: 6px 0px;
        box-shadow: none;
        font-size: 13px;
        -webkit-appearance: none;
        border-radius: 0;
        outline: 0;
    }

    .modal-mask {
        position: fixed;
        z-index: 9998;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, .5);
        display: table;
        transition: opacity .3s ease;
    }

    .modal-wrapper {
        display: table-cell;
        vertical-align: middle;
    }

    .modal-container {
        width: 300px;
        margin: 0px auto;
        padding: 20px 30px;
        background-color: #fff;
        border-radius: 2px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
        transition: all .3s ease;
        font-family: Helvetica, Arial, sans-serif;
    }

    .modal-header h3 {
        margin-top: 0;
        color: #42b983;
    }

    .modal-body {
        margin: 20px 0;
    }

    .modal-enter .modal-container,
    .modal-leave-active .modal-container {
        -webkit-transform: scale(1.1);
        transform: scale(1.1);
    }
</style>
