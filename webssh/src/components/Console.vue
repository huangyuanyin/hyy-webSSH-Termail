<template>
  <div class="console" id="terminal"></div>
</template>
<script>
import Terminal from './Xterm'
export default {
  name: 'Console',
  props: {
    terminal: {
      type: Object,
      default: {}
    }
  },
  data() {
    return {
      term: null,
      terminalSocket: null
    }
  },
  methods: {
    runRealTerminal() {
      console.log('webSocket is finished')
    },
    errorRealTerminal() {
      console.log('error')
    },
    closeRealTerminal() {
      console.log('close')
    }
  },
  mounted() {
    console.log('pid : ' + this.terminal.pid + ' is on ready')
    let terminalContainer = document.getElementById('terminal')
    this.term = new Terminal()
    this.term.open(terminalContainer)
    // open websocket
    this.terminalSocket = new WebSocket('ws://127.0.0.1:3003/terminals/') // 此地址端口号3003需与serve.py文件中的服务器端口号保持一致
    this.terminalSocket.onopen = this.runRealTerminal
    this.terminalSocket.onclose = this.closeRealTerminal
    this.terminalSocket.onerror = this.errorRealTerminal
    this.term.attach(this.terminalSocket)
    this.term._initialized = true
    console.log('mounted is going on')
  },
  beforeDestroy() {
    this.terminalSocket.close()
    this.term.destroy()
  }
}
</script>