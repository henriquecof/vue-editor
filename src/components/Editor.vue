<template>
  <div class="editor">
    <button @click="openfile" class="button is-small is-primary is-outlined">Abrir Arquivo</button>
    <button @click="saveFile" class="button is-small is-info is-outlined">Salvar Arquivo</button>
    <quill-editor v-model="content" :options="editorOption"></quill-editor>
  </div>
</template>

<script>
import hljs from "highlight.js";
import { remote } from "electron";
import "highlight.js/styles/monokai-sublime.css";
import "bulma/css/bulma.min.css";
import fs from "fs";
export default {
  name: "Editor",
  methods: {
    openFile() {
      // eslint-disable-next-line no-unused-vars
      const [filepath] = remote.dialog.showOpenDialog({
        properties: ["openFile"]
      });
      const context = this;
      if (filepath === undefined || !filepath.includes(".escola.js")) {
        alert("Nenhum arquivo selecionado");
        return;
      }
      fs.readFile(filepath, "utf-8", (err, data) => {
        if (err) {
          alert("Um erro ocorreu: " + err.message);
          return;
        }
        context.content = data;
      });
    },
    saveFile() {
      const filenameToSave = remote.dialog.showSaveDialog();
      const filepathToSave = filenameToSave.includes(".escola_js")
        ? filenameToSave
        : `${filenameToSave}.escola_js`;
      fs.writeFile(filepathToSave, this.content, err => {
        if (err) throw err;
        alert("Aqruivo salvo com sucesso");
      });
    }
  },
  data() {
    return {
      content: "",
      editorOption: {
        modules: {
          toolbar: {
            container: [
              ["bold", "italic", "underline", "strike"],
              ["blockquote", "code-block"],
              [{ header: 1 }, { header: 2 }],
              [{ list: "ordered" }, { list: "bullet" }],
              [{ script: "sub" }, { script: "super" }],
              [{ indent: "-1" }, { indent: "+1" }],
              [{ direction: "rtl" }],
              [{ size: ["smal", false, "large", "huge"] }],
              [{ header: [1, 2, 3, 4, 5, 6, false] }],
              [{ font: [] }],
              [{ color: [] }, { background: [] }],
              [{ align: [] }],
              ["clean"],
              ["link", "image", "video"]
            ],
            handlers: { emoji: () => {} }
          },
          syntax: {
            highlight: text => hljs.highlightAuto(text).value
          },

          //plugin drag and drop
          imageDrop: true,
          //plugin resize
          imageResize: {
            modules: ["Resize", "DisplaySize", "Toolbar"]
          },
          "emoji-toolbar": true,
          "emoji-shotname": true
        }
      }
    };
  }
};
</script>


<style>
.quill-editor,
.quill-code {
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
button {
  margin: 5px 5px 5px 5px;
}
</style>