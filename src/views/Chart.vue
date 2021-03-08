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
            <div class="node" :class="item.status|getStatusEnName" :style="item.style" :key="index" @click="handleClick(item)" v-for="(item, index) in charts.nodes">
                <el-popover placement="top-start" :title="item.name" width="300" trigger="hover">
                    <p>副本：{{item.duplicate}}</p>
                    <p>计算任务：{{item.task}}</p>
                    <p>资源：{{item.resource}}</p>
                    <el-button size="mini" type="text" @click="openUrl(item.url)">走一个</el-button>
                    <span slot="reference"><i :class="item.status|getStatusIcon"></i>{{item.name}}</span>
                </el-popover>
            </div>
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
            createTime: '2020-12-07 11:30',
            endTime: '2020-12-09 08:25',
            status: 0,
            url: 'https://element.eleme.cn/#/zh-CN/component/installation',
            resource: 'CPU:3.4GHz RAM:32G GPU:8G', // 资源
            task: '共10个，3运行中，7等待', // 计算任务
            duplicate: '1创建中，1运行中', // 副本
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
            createTime: '2020-12-07 11:30',
            endTime: '2020-12-09 08:25',
            status: 1,
            url: 'https://element.eleme.cn/#/zh-CN/component/installation',
            resource: 'CPU:3.4GHz RAM:32G GPU:8G', // 资源
            task: '共10个，3运行中，7等待', // 计算任务
            duplicate: '2创建中，1运行中', // 副本
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
            createTime: '2020-12-07 11:30',
            endTime: '2020-12-09 08:25',
            status: 2,
            url: 'https://element.eleme.cn/#/zh-CN/component/installation',
            resource: 'CPU:3.4GHz RAM:32G GPU:8G', // 资源
            task: '共10个，3运行中，7等待', // 计算任务
            duplicate: '3创建中，1运行中', // 副本
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
            createTime: '2020-12-07 11:30',
            endTime: '2020-12-09 08:25',
            status: 3,
            url: 'https://element.eleme.cn/#/zh-CN/component/installation',
            resource: 'CPU:3.4GHz RAM:32G GPU:8G', // 资源
            task: '共10个，3运行中，7等待', // 计算任务
            duplicate: '4创建中，1运行中', // 副本
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
    const that = this
    jsPlumb.ready(function() {
      const color = '#a0cfff'
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

      instance.batch(() => {
        const nodes = jsPlumb.getSelector('#canvas .node')
        instance.draggable(nodes)
        for (let i = 0; i < nodes.length; i++) {
          that.charts.nodes[i].endPoints.forEach(ele => {
            instance.addEndpoint(nodes[i], {
              uuid: ele.uuid,
              anchor: ele.anchor,
              isSource: true,
              isTarget: true,
              maxConnections: -1
            })
          })
        }
        that.charts.conections.forEach(ele => {
          instance.connect({
            uuids: ele,
            detachable: false,
            reattach: false
          })
        })
      })

      $('.aside .node').draggable(
        {
          scope: 'plant',
          helper: 'clone',
          containment: $('.container')
        }
      )

      $('#canvas').droppable({
        scope: 'plant',
        drop: function(ev, ui) {
          console.log(ev, ui)

          const id = 'item' + new Date().getTime()
          const html = `<div class="node running" style={ left: "200px", top: "100px"}>
                    <span><i class="el-icon-loading"></i>运行中</span>
                </div>`
          console.info(html)
          $(this).append('<div class="node waiting jtk-managed jtk-draggable jtk-endpoint-anchor jtk-connected" style="left: 100px; top: 200px;"><span data-v-9c9301d8=""><div role="tooltip" id="el-popover-6222" aria-hidden="true" class="el-popover el-popper" tabindex="0" style="width: 300px; display: none;"><div class="el-popover__title">等待中</div><p data-v-9c9301d8="">副本：1创建中，1运行中</p><p data-v-9c9301d8="">计算任务：共10个，3运行中，7等待</p><p data-v-9c9301d8="">资源：CPU:3.4GHz RAM:32G GPU:8G</p><button data-v-9c9301d8="" type="button" class="el-button el-button--text el-button--mini"><!----><!----><span>走一个</span></button></div><span class="el-popover__reference-wrapper"><span data-v-9c9301d8="" class="el-popover__reference" aria-describedby="el-popover-6222" tabindex="0"><i data-v-9c9301d8="" class="el-icon-s-tools"></i>等待中;lg;dlgl;lg;ldsflg;</span></span></span></div>')
          //   $('#' + id).css({
          //     top: ui.position.top + 'px',
          //     left: ui.position.left + 'px'
          //   })
          //   addEndpoints(
          //     id,
          //     ['RightMiddle', 'BottomCenter'],
          //     ['LeftMiddle', 'TopCenter']
          //   )

          /* jsPlumb.addEndpoint(id, { anchors: "TopCenter" });
          jsPlumb.addEndpoint(id, { anchors: "RightMiddle" });
          jsPlumb.addEndpoint(id, { anchors: "BottomCenter" });
          jsPlumb.addEndpoint(id, { anchors: "LeftMiddle" }); */

          /* instance.draggable(jsPlumb.getSelector("#" + id), {
            containment: "parent"
          }); */
          instance.draggable(id, {
            grid: [1, 1]
            // containment: true
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

<style scoped lang="scss">
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
