<template>
  <div>
    ImageEditor
    <button @click="onClickTool('erase')">Erase</button>
    <button>Path tool</button>
    <button>Undo</button>
    <div class="editor-area-content-inner" id="svg000"
      @mouseover="inside = true" @mouseleave="inside = false"
      @mousedown="onMouseDown" @mouseup="down = false;"
      @mousemove="onDrag">

    </div>
  </div>
</template>

<script>
export default {
  name: 'ImageEditor',
  data(){
    return{
      //svg object reference
      svg_container: null,
      svg_image_1: null,

      svg_dfs: null,
      svg_mask1: null,
      svg_rect1: null,
      svg_polyline1: null,

      //Static data
      CANVAS_WIDTH: 400,
      CANVAS_HEIGHT: 400,
      mask1ID: "SvgjsMask1000",
      demoURL: "https://steamuserimages-a.akamaihd.net/ugc/940586530515504757/CDDE77CB810474E1C07B945E40AE4713141AFD76/",

      //UI controller
      toolType: '',
      inside: false,
      down: false,
    }
  },
  mounted(){
    this.init();
  },
  methods:{
    init(){
      console.log("init");
      this.initSvgContianer();
      this.createImage(this.demoURL);
      this.createDefs();
      // this.demoSvg();
    },
    initSvgContianer(){
      this.svg_container = this.createSVGNode('svg', {
        'width': `${this.CANVAS_WIDTH}px`,
        'height': `${this.CANVAS_HEIGHT}px`
      })
      document.getElementById("svg000").appendChild(this.svg_container);
    },
    createImage(url){
      this.svg_image_1 = this.createSVGNode("image", {
        "href": url,
        "height": this.CANVAS_HEIGHT,
        "width": this.CANVAS_WIDTH
      })
      this.svg_container.appendChild(this.svg_image_1);
    },
    createDefs(){
      this.svg_dfs = this.createSVGNode('defs');
      this.svg_mask1 = this.createSVGNode('mask', {'id': `${this.mask1ID}`})
      this.svg_rect1 = this.createSVGNode('rect', {
        'width': this.CANVAS_WIDTH,
        'height': this.CANVAS_HEIGHT,
        'fill': '#ffffff'
      })
      this.polyline1 = this.createSVGNode('polyline', {
        'points': '',
        'fill': 'none',
        'stroke-width': '35',
        'stroke': '#000000',
        'stroke-linecap': 'round',
        'stroke-linejoin': 'round'
      })
      this.svg_mask1.appendChild(this.svg_rect1);
      this.svg_mask1.appendChild(this.polyline1);
      this.svg_dfs.appendChild(this.svg_mask1);
      this.svg_container.appendChild(this.svg_dfs);
      this.svg_image_1.setAttribute('mask', `url("#${this.mask1ID}")`)
    },
    createSVGNode(tag, attrs){
      const svgNode = document.createElementNS("http://www.w3.org/2000/svg", tag);
      if(attrs){
        for (const [key, value] of Object.entries(attrs)) {
          svgNode.setAttribute(key, value);
        }
      }
      return svgNode;
    },
    onClickTool(toolName){
      if(this.toolType === toolName){
        toolName = '';
      }else{
        this.toolType = toolName;
      }
    },
    onMouseDown(e){
      this.down = true;
      console.log(e.offsetX, e.offsetY);
      // const pAttr = String(this.polyline1.getAttribute('points'));
      // this.polyline1.setAttribute('points', pAttr + ` ${e.offsetX},${e.offsetY}`)
    },
    onDrag(e){
      if(this.inside && this.down){
        console.log(e);
        const curX = e.offsetX;
        const curY = e.offsetY;
        console.log(curX, curY);
        const pAttr = String(this.polyline1.getAttribute('points'));
        this.polyline1.setAttribute('points', pAttr + ` ${e.offsetX},${e.offsetY}`)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.editor-area-content-inner{
  border: 1px solid #cbcbcb;
  background-image: url("data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA3LjkzNyA3LjkzOCIgaGVpZ2h0PSIzMCIgd2lkdGg9IjMwIj48cGF0aCBwYWludC1vcmRlcj0ic3Ryb2tlIGZpbGwgbWFya2VycyIgZD0iTS4wMTQuMDE0SDMuOTdWMy45N0guMDE0ek0zLjk3IDMuOTY4aDMuOTU0djMuOTU1SDMuOTd6IiBmaWxsPSIjZWVlZmYwIi8+PC9zdmc+");
  background-size: 25px;
  width: 400px;
  height: 400px;
  font-size: 0;
  margin: 0 auto;
}
</style>
