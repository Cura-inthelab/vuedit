<template>
  <div class="">
    <div class="tags buttons">
      <span  v-for='command in commandRelation' :key="command.cmd">
        <code :class=supported(command) :title=command.desc @keydown="event.preventDefault();" @click="doCommand(command.cmd)">
          <i :class=icon(command) ></i> {{command.cmd}}
        </code>
      </span>
    </div>
  </div>
</template>

<script>
import CommandProperties from '../CommandProperties'

export default {
  name: 'command-menu',
  computed: {},
  data () {
    return {
      commandRelation: {
        cmd: '1234',
        desc: '3333',
        icon: '44'
      }
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
        alert(`${cmd.cmd} is not supported in your browser`)
        return
      }
      let val = (typeof cmd.val !== 'undefined') ? prompt('Value for ' + cmd.cmd + '?', cmd.val) : ''
      document.execCommand(cmd.cmd, false, (val || '')) // Thanks to https://codepen.io/bluestreak for finding this bug
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
  }
}
</script>

<style scoped>

</style>
