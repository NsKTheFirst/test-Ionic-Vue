<template>
  <ion-app>
    <ion-page class="show-page">
      <ion-header>
        <ion-toolbar>
          <ion-title>Ionic 4 + Vue Application</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-content class="content" padding>
        <canvas ref="canvas" id='drawing-pad' width='300' height='300'>
          This is an interactive drawing pad.
        </canvas>
        <div class="colorChoice">
          <span class="black" style="background: #000" @click="changeColor($event)"></span>
          <span class="red" style="background: #ff0000" @click="changeColor($event)"></span>
          <span class="blue" style="background: #0000FF" @click="changeColor($event)"></span>
          <span class="yellow" style="background: #ffe900" @click="changeColor($event)"></span>
        </div>
        <div>
          <ion-button @click='resetCanvas'>Reset Canvas</ion-button>
        </div>
        <ion-button @click="goToHome" full>Go Home</ion-button>
      </ion-content>
    </ion-page>
  </ion-app>
</template>

<script>
  export default {
    name: "Page2",
    data() {
      return {
        canvas: null,
        context: null,
        isDrawing: false,
        startX: 0,
        startY: 0,
        points: [],
        ongoingTouches: [],
        oPos: null,
        oCtx: null,
        drawingColor: "#000"
      }
    },
    mounted() {
      this.canvas = this.$refs.canvas
      this.context = this.canvas.getContext("2d");
      this.canvas.addEventListener("touchstart", this.downDrawligne);
      this.canvas.addEventListener("touchend", this.upDrawligne);
      this.canvas.addEventListener("touchmove", this.moveDrawligne);
    },
    methods: {
      goToHome() {
        this.$router.push('/')
      },
      moveDrawligne(oEvent){
    // var oCanvas = oEvent.currentTarget,
      this.oCtx = null, this.oPos = null;
    if(this.canvas.bDraw ==false){
      return false;
    }//if
    this.oPos = this.getPosition(oEvent, this.canvas);
    this.oCtx = this.canvas.getContext('2d');

    //dessine
    this.oCtx.strokeStyle = this.drawingColor;
    this.oCtx.lineWidth = 3;
    this.oCtx.beginPath();
    this.oCtx.moveTo((this.canvas.posX), this.canvas.posY);
    this.oCtx.lineTo(this.oPos.posX, this.oPos.posY);
    this.oCtx.stroke();

    this.canvas.posX = this.oPos.posX;
    this.canvas.posY = this.oPos.posY;
  }, //fct

  getPosition(oEvent, canvas){
    var oRect = canvas.getBoundingClientRect(),
      oEventEle = oEvent.changedTouches? oEvent.changedTouches[0]:oEvent;
    return {
      posX : (oEventEle.clientX - oRect.left) / (oRect.right - oRect.left) * canvas.width,
      posY : (oEventEle.clientY - oRect.top) / (oRect.bottom - oRect.top) * canvas.height
    };//
  },//fct

  downDrawligne(oEvent){
    oEvent.preventDefault();
    // var  oCanvas = oEvent.currentTarget,
      this.oPos = this.getPosition(oEvent, this.canvas);
    this.canvas.posX = this.oPos.posX;
    this.canvas.posY = this.oPos.posY;
    this.canvas.bDraw = true;
    // capturer(false);
  },//fct

  upDrawligne(oEvent){
    // var oCanvas = oEvent.currentTarget;
    this.canvas.bDraw = false;
    // capturer(true);
  },
      // mousedown(e) {
      //   let rect = this.canvas.getBoundingClientRect();
      //   let x = e.clientX - rect.left;
      //   let y = e.clientY - rect.top;
      //
      //   this.isDrawing = true;
      //   this.startX = x;
      //   this.startY = y;
      //   this.points.push({
      //     x: x,
      //     y: y
      //   });
      // },
      // mousemove(e) {
      //   let rect = this.canvas.getBoundingClientRect();
      //   let x = e.clientX - rect.left;
      //   let y = e.clientY - rect.top;
      //
      //   if (this.isDrawing) {
      //     this.context.beginPath();
      //     this.context.moveTo(this.startX, this.startY);
      //     this.context.lineTo(x, y);
      //     this.context.lineWidth = 1;
      //     this.context.lineCap = 'round';
      //     this.context.strokeStyle = "rgba(0,0,0,1)";
      //     this.context.stroke();
      //
      //     this.startX = x;
      //     this.startY = y;
      //
      //     this.points.push({
      //       x: x,
      //       y: y
      //     });
      //   }
      // },
      // mouseup(e) {
      //   this.isDrawing = false;
      //   if (this.points.length > 0) {
      //     localStorage['points'] = JSON.stringify(this.points);
      //   }
      // },
      resetCanvas() {
        this.canvas.width = this.canvas.width;
        this.points.length = 0; // reset points array
      },
      changeColor(e){
        if (e.target.className === "black"){
          this.drawingColor = "#000"
        }
        if (e.target.className === "red"){
          this.drawingColor = "#ff0000"
        }
        if (e.target.className === "blue"){
          this.drawingColor = "#0000FF"
        }
        if (e.target.className === "yellow"){
          this.drawingColor = "#ffe900"
        }
      }
    }
  }
</script>

<style scoped>
  canvas {
    border: 1px solid red;
    cursor: crosshair;
  }
  .colorChoice{
    display: flex;
    flex-direction: row;
    justify-content: center
  }
  span{
    width: 40px;
    height: 40px
  }
</style>
