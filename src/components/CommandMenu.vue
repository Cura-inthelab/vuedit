<template>
  <div class="">
    <div class="tags buttons">
      <button :class=supported(command) v-for='command in commandRelation' :key="command.cmd">
        <code  :title=command.desc @keydown="event.preventDefault();" @click="doCommand(command.cmd)">
          <i :class=icon(command) ></i> {{command.desc}}
        </code>
      </button>
      <button>
        <code title="표 만들기" @keydown="event.preventDefault();" @click="createTable()">
          <i  ></i> 표 만들기
        </code>
      </button>
    </div>
    <div id="vuedit" contenteditable="true" @input="onInputChange($event)" @keydown="onKeyDown($event)">
      <div>some word word</div>
      <div>some word word</div>
      <div>some word word</div>
    </div>
    <div id="html-output">{{this.htmlOutput}}</div>
  </div>
</template>

<script>
import CommandProperties from '../CommandProperties'

export default {
  name: 'command-menu',
  components: {},
  computed: {

  },
  data () {
    return {
      commandRelation: {
        cmd: '1234',
        desc: '3333',
        icon: '44'
      },
      htmlOutput: ''
    }
  },
  methods: {
    icon (cmd) {
      return (typeof cmd.icon !== 'undefined') ? 'fa fa-' + cmd.icon : ''
    },
    supported (cmd) {
      let css = document.queryCommandSupported(cmd.cmd) ? 'btn-success' : 'btn-error'
      return css
    },
    doCommand (cmdKey) {
      console.log(`${cmdKey} is executing`)
      let cmd = this.commandRelation[cmdKey]
      if (this.supported(cmd) === 'btn-error') {
        alert(`${cmd.cmd}은 현재 브라우저에서 지원하지 않습니다.`)
        return
      }
      let val = (typeof cmd.val !== 'undefined') ? prompt(cmd.desc + '에 필요한 값을 입력해주십시요.', cmd.val) : ''
      document.execCommand(cmd.cmd, false, (val || ''))
    },
    onInputChange (e) {
      this.htmlOutput = e.target.innerHTML
    },
    onKeyDown (e) {
      console.log(e.keyCode)
      if (e.keyCode === 13) {
        document.execCommand('insertHTML', false, '<p><p>')
      }
    },
    createTable(){
      let row = prompt('줄');
      let col = prompt('칸');

      let tableHTML = '<table>';

      for (let k = 0; k < row; k++) {
        tableHTML += '<tr>'
        for (let m = 0; m < col; m++) {
          tableHTML += '<td></td>'
        }
        tableHTML += '</tr>'
      }
      tableHTML += '</table>'

      document.execCommand('insertHTML', false, tableHTML)
    }
  },
  created () {
    let commands = CommandProperties.commandList
    let vueContext = this
    commands.map(function (command, i) {
      vueContext.commandRelation[command.cmd] = command
    })
  },
  mounted () {
    this.htmlOutput = document.getElementById('vuedit').innerHTML
  }
}
</script>

<style>

  #vuedit{
    height: 100%;
    min-height: 500px;
  }

  .btn-error{
    display: none;
  }
  table {
    width: 100%;
    height: 100%;
    border: 1px solid black;
    border-collapse: collapse;
  }

  tr, td {
    border: 1px solid black;
    border-collapse: collapse;
  }
</style>
