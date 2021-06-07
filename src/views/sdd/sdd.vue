<template>
  <div>
    <a-button type="primary" @click="a">
      {{b}}
    </a-button>
    <a-button>Default</a-button>
    <a-button type="dashed">
      Dashed
    </a-button>
    <a-button type="danger">
      Danger
    </a-button>
    <a-config-provider :auto-insert-space-in-button="false">
      <a-button type="primary">
        按钮
      </a-button>
    </a-config-provider>
    <a-button type="primary">
      按钮
    </a-button>
    <a-button type="link">
      Link
    </a-button>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        b:"我爱宋丹丹"
      }
    },
    methods: {
      a() {
         this.b="宋丹丹爱我";
      }
    }

  }
</script>