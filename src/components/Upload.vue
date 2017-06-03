<template>
    <div style="display: none">
        <input type="file" id="upload-tag"/>
    </div>
</template>

<script>

    import AWS from 'aws-sdk';
    import $ from 'jquery';

    /*
     Amazon Cognito
     */

    const Region = "*** your setting ***";
    const PoolId = "*** your setting ***";
    const Bucket = "*** your setting ***";

    const s3Client = () => {
        AWS.config.region = Region;
        AWS.config.credentials = new AWS.CognitoIdentityCredentials({IdentityPoolId: PoolId});
        AWS.config.credentials.get(err => {
            !err ? console.log("Cognito Identify Id: " + AWS.config.credentials.identityId) : null;
        });
        return new AWS.S3({params: {Bucket: Bucket}});
    };

    export default {
        props: [
            'action'
        ],
        data () {
            return {
                fileUrl: "",
            }
        },
        watch: {
            fileUrl(val){
                const data = {
                    'src': this.fileUrl,
                };
                this.$emit('callback', data);
            },
            action(val){
                val ? document.getElementById("upload-tag").click() : "";
            },
        },
        methods: {
            getFileName(fileName) {
                return "https://s3-ap-northeast-1.amazonaws.com/" + Bucket + "/" + fileName;
            }
        },
        mounted() {
            const self = this;

            $('#upload-tag').on("change", () => {
                const file = $('#upload-tag').prop("files")[0];
                const timestamp = new Date().getTime();
                const f = file.name.split('.');
                const filename = "file" + timestamp + "." + f[f.length - 1].toLowerCase();

                s3Client().putObject({Key: filename, ContentType: file.type, Body: file, ACL: "public-read"},
                    (err, data) => {
                        self.fileUrl = self.getFileName(filename);
                        data === null ? console.log("error") : "";
                    });
            });
        },
    }
</script>

