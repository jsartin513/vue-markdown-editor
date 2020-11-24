<template>
  <div class="markdownEditor">
    <div class="panel editor">
      <div class="editorContent">
        <h2>Editor Panel</h2>
        <textarea v-model="markdown" rows="30" cols="50"> </textarea>
      </div>
    </div>

    <div class="panel viewer">
      <div class="viewerContent">
        <h2>Viewer Panel</h2>
        <div v-html=markdownToHtml />
        <!-- <p>{{markdownToHtml}}</p> -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Markdown Editor",
  props: {},
  data: function () {
    return {
      markdown: "",
    };
  },
  computed: {
    markdownToHtml() { 
        const lines = this.markdown.split("\n");
        return lines.map((line) => { 
            var hashCount = 0;
            var relevant_chars = '';
            var still_hash_symbols = true;
            line.split('').map((char) => {
                if(still_hash_symbols &&  char === '#'){
                    hashCount ++;
                }
                else {
                  relevant_chars += char;
                }
            })
            var headerOpenerTag = '<H'+ hashCount.toString() + '>';
            var headerCloserTag = '<H'+ hashCount.toString() + '/>';
            var headerText = headerOpenerTag + relevant_chars + headerCloserTag;
            return headerText;
        });
        },
  },
};
</script>
<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.markdownEditor {
  display: flex;
  flex-direction: row;
}

.panel {
  display: flex;
  flex: 1;
}
</style>
