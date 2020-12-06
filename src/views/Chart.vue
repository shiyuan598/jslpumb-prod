<template>
  <div class="container" id="canvas">
    <div class="node running" @click="handleClick" @mouseenter="handleMouseenter"><i class="el-icon-loading"></i>node1</div>
    <div class="node error">
      <el-popover placement="top-start" title="标题" width="200" trigger="hover" content="这是一段内容,这是一段内容,这是一段内容,这是一段内容。">
        <p>这是一段内容这是一段内容</p>
        <el-button size="mini" type="text" @click="openUrl">走一个</el-button>
        <span slot="reference"><i class="el-icon-circle-close"></i>node2</span>
      </el-popover>
    </div>
    <div class="node waiting"><i class="el-icon-s-tools"></i>node3</div>
    <div class="node success"><i class="el-icon-circle-check"></i>node4</div>
    <div class="node error"><i class="el-icon-circle-close"></i>node5</div>
  </div>
</template>

<script>
export default {
  name: 'Chart',
  data() {
    return {}
  },
  mounted() {
    jsPlumb.ready(function() {
      const color = '#25CCF7'
      const instance = jsPlumb.getInstance({
        Connector: ['Bezier', { curviness: 50 }],
        ConnectionOverlays: [
          [
            'Arrow',
            {
              location: 1,
              visible: true,
              width: 11,
              length: 11,
              id: 'ARROW',
              events: {
                click: function() {
                  alert('you clicked on the arrow overlay')
                }
              }
            }
          ]
        ],
        DragOptions: { cursor: 'pointer', zIndex: 2000 },
        PaintStyle: { stroke: color, strokeWidth: 2 },
        EndpointStyle: { radius: 5, fill: color },
        HoverPaintStyle: { stroke: '#ec9f2e' },
        EndpointHoverStyle: { fill: '#ec9f2e' },
        Container: 'canvas'
      })

      instance.batch(function() {
        const nodes = jsPlumb.getSelector('#canvas .node')
        instance.draggable(nodes)
        const arrowCommon = { foldback: 0.7, fill: color, width: 14 }
        // use three-arg spec to create two different arrows with the common values:
        const overlays = [['Arrow', { location: 1 }, arrowCommon]]
        for (var i = 0; i < nodes.length; i++) {
          instance.addEndpoint(nodes[i], {
            uuid: 'top-' + i,
            anchor: 'Top',
            isSource: true,
            isTarget: true,
            maxConnections: -1
          })
          instance.addEndpoint(nodes[i], {
            uuid: 'bottom-' + i,
            anchor: 'Bottom',
            isSource: true,
            isTarget: true,
            maxConnections: -1
          })
          instance.addEndpoint(nodes[i], {
            uuid: 'left-' + i,
            anchor: 'Left',
            isSource: true,
            isTarget: true,
            maxConnections: -1
          })
          instance.addEndpoint(nodes[i], {
            uuid: 'right-' + i,
            anchor: 'Right',
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
    handleClick() {
      console.info('click')
    },
    handleMouseenter() {
      console.info('mouseenter')
    },
    openUrl() {
      window.open('https://element.eleme.cn/#/zh-CN/component/installation')
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
    // width: 60px;
    height: 30px;
    line-height: 30px;
    text-align: center;
    // border: 1px solid #f8efba;
    border-radius: 3px;
    padding: 0 8px;
    // &:hover {
    //   border: 1px #eab543 solid;
    //   cursor: pointer;
    // }
  }
  .node:nth-child(1) {
    left: 30px;
  }
  .node:nth-child(2) {
    left: 200px;
  }
  .node:nth-child(3) {
    left: 400px;
  }
  .node:nth-child(4) {
    left: 600px;
  }
  .node:nth-child(5) {
    left: 800px;
  }
  $runningColor: #409eff;
  .running {
    color: $runningColor;
    border: 1px solid $runningColor;
    background-color: #ecf5ff;
    &:hover {
      color: #fff;
      background-color: $runningColor;
    }
  }
  $successColor: #67c23a;
  .success {
    color: $successColor;
    border: 1px solid $successColor;
    background-color: #f0f9eb;
    &:hover {
      color: #fff;
      background-color: $successColor;
    }
  }
  $errorColor: #f56c6c;
  .error {
    color: $errorColor;
    border: 1px solid $errorColor;
    background-color: #fef0f0;
    &:hover {
      color: #fff;
      background-color: $errorColor;
    }
  }
  $waitingColor: #909399;
  .waiting {
    color: $waitingColor;
    border: 1px solid $waitingColor;
    background-color: #f4f4f5;
    &:hover {
      color: #fff;
      background-color: $waitingColor;
    }
  }
}
</style>
