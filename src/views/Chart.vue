<template>
  <div class="container" id="canvas">
    <div class="node" @click="handleClick" @mouseenter="handleMouseenter">node1</div>
    <div class="node">node2</div>
    <div class="node">node3</div>
    <div class="node">node4</div>
    <div class="node">node5</div>
  </div>
</template>

<script>
export default {
  name: 'Chart',
  data () {
    return {}
  },
  mounted () {
    jsPlumb.ready(function () {
      const color = '#25CCF7'
      const instance = jsPlumb.getInstance({
        Connector: ['Bezier', { curviness: 50 }],
        DragOptions: { cursor: 'pointer', zIndex: 2000 },
        PaintStyle: { stroke: color, strokeWidth: 2 },
        EndpointStyle: { radius: 5, fill: color },
        HoverPaintStyle: { stroke: '#ec9f2e' },
        EndpointHoverStyle: { fill: '#ec9f2e' },
        Container: 'canvas'
      })

      instance.batch(function () {
        const nodes = jsPlumb.getSelector('#canvas .node')
        instance.draggable(nodes)
        const arrowCommon = { foldback: 0.7, fill: color, width: 14 }
        // use three-arg spec to create two different arrows with the common values:
        const overlays = [
          ['Arrow', { location: 1 }, arrowCommon]
        ]
        for (var i = 0; i < nodes.length; i++) {
          instance.addEndpoint(nodes[i], {
            uuid: 'bottom-' + i,
            anchor: 'Bottom',
            isSource: true,
            isTarget: true,
            maxConnections: -1
          })
        }
        instance.connect({
          uuids: ['bottom-1', 'bottom-3'],
          overlays: overlays,
          detachable: true,
          reattach: false
        })
      })
    })
  },
  methods: {
    handleClick () {
      console.info('click')
    },
    handleMouseenter () {
      console.info('mouseenter')
    }
  }
}
</script>

<style scoped lang="scss">
.container {
  position: relative;
  border: 1px solid #efefef;
  height: 60%;
  width: 60%;
  top: 10%;
  left: 50%;
  transform: translate(-50%, 0);
  display: flex;
  justify-content: space-around;
  padding: 30px;
  .node {
    position: absolute;
    box-sizing: content-box;
    height: 30px;
    line-height: 30px;
    text-align: center;
    border: 1px solid #f8efba;
    border-radius: 3px;
    padding: 0 8px;
    &:hover {
      border: 1px #eab543 solid;
      cursor: pointer;
    }
  }
  .node:nth-child(1) {
    left: 30px;
  }
  .node:nth-child(2) {
    left: 120px;
  }
  .node:nth-child(3) {
    left: 210px;
  }
  .node:nth-child(4) {
    left: 300px;
  }
  .node:nth-child(5) {
    left: 390px;
  }
}
</style>
