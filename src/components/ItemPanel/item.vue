<template>
  <ul>
    <li
      v-for="(item,index) in list"
      :key="index"
      class="getItem"
      :data-shape="item.shape"
      :data-type="item.type"
      :data-size="item.size"
      draggable
      @dragstart="handleDragstart"
      @dragend="handleDragEnd($event,item)"
    >
      <span class="pannel-type-icon" :style="{background:'url('+item.image+')'}"></span>
      {{item.name}}
    </li>
  </ul>
</template>

<script>
import eventBus from "@/utils/eventBus";
import okSvg from "@/assets/icons/ok.svg";
import bgImg from "@/assets/bg1.jpg";
import RedisImg from "@/assets/redis.png";
import nginxSvg from "@/assets/icons/d.svg";
import redisSvg from "@/assets/icons/redis.svg";
import NodeSvg from "@/assets/icons/nodejs.svg";
import computerSvg from "@/assets/icons/computer.svg";
import oracleSvg from "@/assets/icons/oracle.svg";
import mysqlSvg from "@/assets/icons/mysql.svg";
import smartphoneSvg from "@/assets/icons/smartphone.svg";
import tomcatSvg from "@/assets/icons/tomcat.svg";
export default {
  data() {
    return {
      page: null,
      command: null,
      offsetX: 0,
      offsetY: 0,
      list: [
      {
          name: "Mysql",
          label: "",
          size: "64*48",
          type: "node",
          x: 0,
          y: 0,
          shape: "customNode",
          color: "#1890ff",
          image:
            mysqlSvg,
          backImage: mysqlSvg,
          inPoints: [[0, 0.5]],
          outPoints: [[1, 0.5]]
        },
        {
          name: "Oracle",
          label: "",
          size: "64*48",
          type: "node",
          x: 0,
          y: 0,
          shape: "customNode",
          color: "#1890ff",
          image:
            oracleSvg,
          backImage: oracleSvg,
          inPoints: [[0, 0.5]],
          outPoints: [[1, 0.5]]
        },
        {
          name: "Redis",
          label: "",
          size: "64*48",
          type: "node",
          x: 0,
          y: 0,
          shape: "customNode",
          color: "#1890ff",
          image:
            redisSvg,

          backImage: redisSvg,
          inPoints: [[0, 0.5]],
          outPoints: [[1, 0.5]]
        },

        {
          name: "Nginx",
          label: "",
          size: "64*48",
          type: "node",
          x: 0,
          y: 0,
          shape: "customNode",
          color: "#1890ff",
          image:
            nginxSvg,
          stateImage: okSvg,
          backImage: bgImg,
          inPoints: [[0, 0.5]],
          outPoints: [[1, 0.5]]
        },
        {
          name: "Nodejs",
          label: "",
          size: "64*64",
          type: "node",
          x: 0,
          y: 0,
          shape: "customNode",
          color: "#1890ff",
          image:
            NodeSvg,

          backImage: NodeSvg,
          inPoints: [[0, 0.5]],
          outPoints: [[1, 0.4], [1, 0.6]]
        },
        {
          name: "Tomcat",
          label: "",
          size: "64*64",
          type: "node",
          x: 0,
          y: 0,
          shape: "customNode",
          color: "#1890ff",
          image:
            tomcatSvg,

          backImage: tomcatSvg,
          inPoints: [[0, 0.5]],
          outPoints: [[1, 0.4], [1, 0.6]]
        },
        {
          name: "PC",
          label: "",
          size: "64*48",
          type: "node",
          x: 0,
          y: 0,
          shape: "customNode",
          color: "#1890ff",
          image:
            computerSvg,
          backImage: computerSvg,
          inPoints: [[0, 0.5]],
          outPoints: [[1, 0.5]]
        },
        {
          name: "Smartphone",
          label: "",
          size: "64*48",
          type: "node",
          x: 0,
          y: 0,
          shape: "customNode",
          color: "#1890ff",
          image:
            smartphoneSvg,
          backImage: smartphoneSvg,
          inPoints: [[0, 0.5]],
          outPoints: [[1, 0.5]]
        },
        {
          name: "动画开始节点",
          label: "动画开始",
          size: "170*34",
          type: "node",
          x: 0,
          y: 0,
          shape: "customNode",
          color: "#1890ff",
          image:
            "https://gw.alipayobjects.com/zos/rmsportal/czNEJAmyDpclFaSucYWB.svg",
          stateImage: okSvg,
          inPoints: [[0, 0.5]],
          outPoints: [[1, 0.5]],
          isDoingStart: true
        },
        {
          name: "动画结束节点",
          label: "动画结束",
          size: "170*34",
          type: "node",
          x: 0,
          y: 0,
          shape: "customNode",
          color: "#1890ff",
          image:
            "https://gw.alipayobjects.com/zos/rmsportal/czNEJAmyDpclFaSucYWB.svg",
          stateImage: okSvg,
          inPoints: [[0, 0.5]],
          outPoints: [[1, 0.5]],
          isDoingEnd: true
        }
      ]
    };
  },
  created() {
    this.bindEvent();
  },
  methods: {
    handleDragstart(e) {
      this.offsetX = e.offsetX;
      this.offsetY = e.offsetY;
    },
    handleDragEnd(e, item) {
      let data = {};
      Object.assign(data, item);
      data.offsetX = this.offsetX;
      data.offsetY = this.offsetY;
      if (this.page) {
        const graph = this.page.graph;
        // const size = e.target.dataset.size.split("*");
        const xy = graph.getPointByClient(e.x, e.y);
        data.x = xy.x;
        data.y = xy.y;
        data.size = item.size.split("*");
        data.type = "node";
        this.command.executeCommand("add", [data]);
      }
    },
    bindEvent() {
      eventBus.$on("afterAddPage", page => {
        this.page = page;
        this.command = page.command;
      });
    }
  }
};
</script>

<style scoped>
.itempannel {
  height: 100%;
  position: absolute;
  left: 0px;
  z-index: 2;
  background: #f7f9fb;
  width: 200px;
  padding-top: 8px;
  border-right: 1px solid #e6e9ed;
}
.itempannel ul {
  padding: 0px;
  padding-left: 16px;
}
.itempannel li {
  color: rgba(0, 0, 0, 0.65);
  border-radius: 4px;
  width: 160px;
  height: 48px;
  line-height: 26px;
  padding-left: 8px;
  border: 1px solid rgba(0, 0, 0, 0);
  list-style-type: none;
}
.itempannel li:hover {
  background: white;
  border: 1px solid #ced4d9;
  cursor: move;
}

.itempannel .pannel-type-icon {
  width: 32px;
  height: 32px;
  display: inline-block;
  vertical-align: middle;
  margin-right: 8px;
}
</style>