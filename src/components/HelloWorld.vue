<template>
  <div id="3d-graph"></div>
</template>

<script>
import ForceGraph3D from '3d-force-graph';
import SpriteText from 'three-spritetext';
import nodes from '/src/assets/nodes.json';
export default {
  name: 'HelloWorld',
  created() {
    var time = null;
    var prevTime = null;
    var myGraph = ForceGraph3D({
      controlType: 'trackball',
    });
    this.$nextTick(()=>{
      myGraph(document.getElementById('3d-graph')).graphData(nodes)
      .nodeLabel('id')
      .nodeAutoColorBy('group')
      .showNavInfo(false)
      .onNodeClick((node) => {
        prevTime = time;
        time = new Date().getSeconds();
        if(prevTime == time){
          console.log('double click',node);
        }
      })
      .onNodeRightClick((node,event) => {
        console.log(node, event);
      })
      .onNodeDrag((event) => {
        console.log(event);
      })
      .onNodeDragEnd(node => {
          node.fx = node.x;
          node.fy = node.y;
          node.fz = node.z;
      })
      .linkThreeObjectExtend(true)
      .nodeThreeObject(node => {
          const sprite = new SpriteText(node.id);
          sprite.material.depthWrite = false; // make sprite background transparent
          sprite.color = node.color;
          sprite.textHeight = 8;
          return sprite;
      });
      myGraph.pauseAnimation();
      myGraph.resumeAnimation();
    })
  }
}
</script>

<style scoped>

</style>
