<template>
    <div class="editor">
        <quill-editor
            v-model="content"
            :options="editorOption">
        </quill-editor>
    </div>
        
</template>

<script>
export default {
    name: "Editor",
    methods: {},
    data() {
        return {
            content: "",
            editorOption: {
                modules: {
                    toolbar: {
                        container: [
                            ["bold", "italic", "underline", "strike"],
                            ["blockquote", "code-block"],
                            [{header: 1}, {header: 2}],
                            [{list: "ordered"}, {list: "bullet"}],
                            [{script: "sub"}, {script: "super"}],
                            [{indent: "-1"}, {indent: "+1"}],
                            [{direction:"rtl"}],
                            [{size:["smal", false, "large", "huge"]}],
                            [{header: [1,2,3,4,5,6, false]}],
                            [{font:[]}],
                            [{color:[]}, {background: []}],
                            [{align:[]}],
                            ["clean"],
                            ["link", "image", "video"]


                        ],
                    },
                }

            }
        }
    }
    };
</script>


<style>
.quill-editor, .quill-code {
    width: 100%;
    height: 90vh;
}
.quill-code {
    height: auto;
    border: none;
}

.quill-code > .title {
    border: 1px solid #ccc;
    border-left: none;
    height: 3em;
    line-height: 3em;
    text-indent: 1rem;
    font-weight: bold;
}

</style>