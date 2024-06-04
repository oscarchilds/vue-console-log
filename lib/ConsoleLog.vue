<script setup lang="ts">
import { ref } from 'vue'
import LogType from './data/LogType';
import LogData from './data/LogData';

const logs = ref<LogData[]>([])

function newFunction(oldFunction: Function, type: LogType) {
  return function (...args: any[]) {
    oldFunction.apply(console, args)
    args.forEach(x => {
      logs.value.push({
        type: type,
        message: x
      })
    })
  }
}

console.log = newFunction(console.log, LogType.log)
console.error = newFunction(console.error, LogType.error)

console.log('hello from code')
console.log('hello from code2', 'second arg')
console.log({ firstField: 'helloField', anObject: { objectName: 'helloObject' }})
console.error('error')
</script>

<template>
  <div class="console-log">
    <div
      v-for="(log, key) in logs"
      :key="key"
      class="line"
      :class="LogType[log.type]"
    >
      <span>
        {{ log.message }}
      </span>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.console-log {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;

  .line {
    border-bottom: 1px solid gray;
    padding: 4px 0;

    &.error {
      background-color: #E57373;
    }

    span {
      padding: 0 5px;
    }
  }
}
</style>