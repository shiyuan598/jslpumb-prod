<template>
    <div class="container">
        <div class="aside">
            <div class="warp">
                <div class="node running">
                    <span><i class="el-icon-loading"></i>运行中</span>
                </div>
            </div>
            <div class="warp">
                <div class="node waiting">
                    <span><i class="el-icon-s-tools"></i>等待中</span>
                </div>
            </div>

            <div class="warp">
                <div class="node success">
                    <span><i class="el-icon-circle-check"></i>成功</span>
                </div>
            </div>

            <div class="warp">
                <div class="node error">
                    <span><i class="el-icon-circle-close"></i>失败</span>
                </div>
            </div>

        </div>
        <div class="main" id="canvas">
        </div>
    </div>
</template>

<script>
export default {
  name: 'Chart',
  data() {
    return {
      charts: {
        nodes: [
          {
            id: 'node1',
            name: '等待中',
            status: 0,
            style: { // 位置参数
              left: '180px',
              top: '300px'
            },
            endPoints: [
              {
                uuid: 'f16754bf', // 不可重复
                anchor: 'Top' // 枚举值 Top Bottom Left Right
              },
              {
                uuid: 'b30f11d7', // 不可重复
                anchor: 'Right' // 枚举值 Top Bottom Left Right
              }
            ]
          },
          {
            id: 'node2',
            name: '运行中',
            status: 1,
            style: { // 位置参数
              left: '200px',
              top: '30px'
            },
            endPoints: [
              {
                uuid: '8dd11673', // 不可重复
                anchor: 'Bottom' // 枚举值 Top Bottom Left Right
              },
              {
                uuid: '20eb8bb1', // 不可重复
                anchor: 'Right' // 枚举值 Top Bottom Left Right
              }
            ]
          },
          {
            id: 'node3',
            name: '运行成功',
            status: 2,
            style: { // 位置参数
              left: '400px',
              top: '30px'
            },
            endPoints: [
              {
                uuid: '8bd78b7d', // 不可重复
                anchor: 'Bottom' // 枚举值 Top Bottom Left Right
              },
              {
                uuid: '6cc387cf', // 不可重复
                anchor: 'Left' // 枚举值 Top Bottom Left Right
              },
              {
                uuid: '5785e11a', // 不可重复
                anchor: 'Right' // 枚举值 Top Bottom Left Right
              }
            ]
          },
          {
            id: 'node4',
            name: '运行失败',
            status: 3,
            style: { // 位置参数
              left: '600px',
              top: '30px'
            },
            endPoints: [
              {
                uuid: '3641e22b', // 不可重复
                anchor: 'Top' // 枚举值 Top Bottom Left Right
              },
              {
                uuid: '011c53e4', // 不可重复
                anchor: 'Bottom' // 枚举值 Top Bottom Left Right
              },
              {
                uuid: '29e59cde', // 不可重复
                anchor: 'Left' // 枚举值 Top Bottom Left Right
              },
              {
                uuid: '9823df0c', // 不可重复
                anchor: 'Right' // 枚举值 Top Bottom Left Right
              }
            ]
          }
        ],
        conections: [ // 通过endPoints的uuid定义连接
          ['20eb8bb1', '6cc387cf'],
          ['5785e11a', '29e59cde'],
          ['011c53e4', 'f16754bf']
        ]
      }
    }
  },
  filters: {
    getStatusEnName (status) {
      let name = ''
      switch (status) {
        case 0:
          name = 'waiting'
          break
        case 1:
          name = 'running'
          break
        case 2:
          name = 'success'
          break
        case 3:
          name = 'error'
          break
      }
      return name
    },
    getStatusZhName (status) {
      let name = ''
      switch (status) {
        case 0:
          name = '等待中'
          break
        case 1:
          name = '运行中'
          break
        case 2:
          name = '成功'
          break
        case 3:
          name = '失败'
          break
      }
      return name
    },
    getStatusIcon (status) {
      let name = ''
      switch (status) {
        case 0:
          name = 'el-icon-s-tools'
          break
        case 1:
          name = 'el-icon-loading'
          break
        case 2:
          name = 'el-icon-circle-check'
          break
        case 3:
          name = 'el-icon-circle-close'
          break
      }
      return name
    }
  },
  mounted() {
    // const that = this
    jsPlumb.ready(function() {
      const color = '#7AB02C'
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
                }
              }
            }
          ]
        ],
        DragOptions: { cursor: 'pointer', zIndex: 2000 },
        PaintStyle: { stroke: color, strokeWidth: 2 },
        EndpointStyle: { radius: 2, fill: color },
        HoverPaintStyle: { stroke: color },
        EndpointHoverStyle: { fill: color },
        Container: 'canvas'
      })

      // source endpoints
      const sourceEndpoint = {
        endpoint: 'Dot',
        paintStyle: {
          stroke: '#7AB02C',
          fill: 'transparent',
          radius: 3,
          strokeWidth: 1
        },
        isSource: true,
        connector: [
          'Bezier',
          {
            stub: [40, 60],
            gap: 10,
            cornerRadius: 5,
            alwaysRespectStubs: true
          }
        ],
        dragOptions: {},
        overlays: [
          [
            'Label',
            {
              location: [0.5, 1.5],
              label: 'Drag',
              cssClass: 'endpointSourceLabel',
              visible: false
            }
          ]
        ]
      }
      // target endpoints
      const targetEndpoint = {
        endpoint: 'Dot',
        paintStyle: { fill: '#7AB02C', radius: 3 },
        maxConnections: -1,
        dropOptions: { hoverClass: 'hover', activeClass: 'active' },
        isTarget: true,
        overlays: [
          [
            'Label',
            {
              location: [0.5, -0.5],
              label: 'Drop',
              cssClass: 'endpointTargetLabel',
              visible: false
            }
          ]
        ]
      }

      $('.aside .warp').draggable({
        scope: 'plant',
        helper: 'clone',
        containment: $('.container')
      })

      $('#canvas').droppable({
        scope: 'plant',
        drop: function(ev, ui) {
          const left = ui.position.left - 240 + 'px'
          const top = ui.position.top - 15 + 'px'
          const id = 'node' + new Date().getTime()
          const html = `<div id=${id} style="left: ${left}; top: ${top}" + ${ui.helper.html().substr(4)}`
          $(this).append(html)
          instance.addEndpoint(id, sourceEndpoint, {
            anchor: 'RightMiddle',
            uuid: id + 'RightMiddle',
            isSource: true,
            maxConnections: -1
          })
          instance.addEndpoint(id, sourceEndpoint, {
            anchor: 'BottomCenter',
            uuid: id + 'BottomCenter',
            isSource: true,
            maxConnections: -1
          })
          instance.addEndpoint(id, targetEndpoint, {
            anchor: 'LeftMiddle',
            uuid: id + 'LeftMiddle',
            isTarget: true,
            maxConnections: -1
          })
          instance.addEndpoint(id, targetEndpoint, {
            anchor: 'TopCenter',
            uuid: id + 'TopCenter',
            isTarget: true,
            maxConnections: -1
          })
          instance.draggable(id, {
            grid: [1, 1],
            containment: true
          })
        }
      })
    })
  },
  methods: {
    handleClick() {
      console.info('click')
    },
    openUrl() {
      window.open('https://element.eleme.cn/#/zh-CN/component/installation')
    }
  }
}
</script>

<style lang="scss">
.container {
    height: calc(100% - 30px);
    width: calc(100% - 30px);
    padding: 15px;
    display: flex;
    .aside {
        width: 170px;
        margin-right: 15px;
        border: 1px solid #ecf5ff;
        padding: 25px;
        * + * {
            margin-top: 15px;
        }
        .node {
            font-size: 14px;
            box-sizing: content-box;
            width: 120px;
            height: 25px;
            line-height: 25px;
            text-align: center;
            border-radius: 3px;
            padding: 0 8px;
        }
    }
    .main {
        position: relative;
        width: calc(100% - 170px - 15px);
        border: 1px solid #ecf5ff;
        .node {
            font-size: 14px;
            position: absolute;
            box-sizing: content-box;
            height: 30px;
            line-height: 30px;
            text-align: center;
            border-radius: 3px;
            padding: 0 8px;
        }
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
