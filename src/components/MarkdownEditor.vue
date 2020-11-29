<template>
  <div class="markdownEditor">
    <div class="panel editor">
      <div class="editorContent">
        <h2 class='page-header'>Editor Panel</h2>
        <textarea v-model="markdown" rows="30" cols="50"> </textarea>
      </div>
    </div>

    <div class="panel viewer">
      <div class="viewerContent">
        <h2 class='page-header'>Viewer Panel</h2>
        <div v-html=markdownToHtml />
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
        var withinList = false;

        var allText = '';
        lines.map((line) => { 
            var lineText = '';
            var firstChar = line[0];
            if (withinList) {
              // Handle list operation
              if(['*', '-', '+'].findIndex((listChar) => listChar === firstChar) === -1){
                lineText += '</ul>';
                withinList = false;
              }
            }
            if(line === ''){
                lineText += '<div>' +  '<br />' + '</div>';
            }
            else if(line[0] === '#') {
                lineText += '<div>' + this.headerHtml(line) + '</div>';
            }
            else if(['*', '-', '+'].findIndex((listChar) => listChar === firstChar) !== -1) {
                lineText += this.listHtml(line, withinList);
                withinList = true;
            }
            else {
                lineText += '<div>' + this.paragraphHtml(line) + '</div>';
            }
            // console.log(linetext)
            allText += lineText;
        });
        return allText;
    },
  },
  methods: {
    headerHtml: (headerMarkdown) => {
        var hashCount = 0;
        var relevant_chars = '';
        var still_hash_symbols = true;
        headerMarkdown.split('').map((char) => {
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
        headerOpenerTag = '';
        headerCloserTag = '';
        return headerText;
    },
    listHtml: (item, isListStartedAlready) => {
      var listText = '';
      if (!isListStartedAlready){
        listText += '<ul>';
      }
      listText += '<li>' + item + '</li>';
      return listText;
    },
    paragraphHtml: (text) => {
        var paragraphText = '<p>' + text + '</p>';
        return paragraphText;
    }
  }
};
</script>
<style>
.markdownEditor {
  display: flex;
  flex-direction: row;
}

.panel {
  border: solid green;
  border-width: thin;
  display: flex;
  flex: 1;
}
</style>
