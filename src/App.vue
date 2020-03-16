<template>
  <div id="app" class="p-5">
    <h1 class="text-center">Tangram Prototype</h1>
    <h3>Pieces in correct place: {{ score }}/{{ this.outlines.length }}</h3>
    <p>(Currently only works with original orientations)</p>
    <v-stage
      :config="stageConfig"
      class="stage"
      :class="{ success: score == outlines.length }"
    >
      <v-layer>
        <v-line
          v-for="outline in outlines"
          :key="outline.name"
          :config="outline"
        ></v-line>
        <v-line
          v-for="(piece, i) in pieces"
          :key="piece.name"
          :config="piece"
          @click="onClick($event, i)"
          @tap="onTap($event, i)"
          @dragstart="onDragStart($event, i)"
          @dragend="onDragEnd($event, i)"
        ></v-line>
      </v-layer>
    </v-stage>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {

  },
  data() {
    return {
      stageConfig: {
        width: 500,
        height: 500
      },
      snapToPer: 5,
      outlines: [
        {
          name: "LargeRightTriangle1Outline",
          x: 50,
          y: 100,
          offsetX: 50,
          offsetY: 100,
          rotation: 0,
          points: [0, 0, 0, 200, 100, 100],
          stroke: "red",
          strokeWidth: 2,
          closed: true,
          draggable: false,
          dash: [10, 5]
        },
        {
          name: "LargeRightTriangle2Outline",
          x: 100,
          y: 50,
          offsetX: 100,
          offsetY: 50,
          rotation: 0,
          points: [0, 0, 200, 0, 100, 100],
          stroke: "red",
          strokeWidth: 2,
          closed: true,
          draggable: false,
          dash: [10, 5]
        },
        {
          name: "MediumRightTriangle1Outline",
          x: 175,
          y: 175,
          offsetX: -25,
          offsetY: -25,
          rotation: 0,
          points: [0, 0, 0, -100, -100, 0],
          stroke: "red",
          strokeWidth: 2,
          closed: true,
          draggable: false,
          dash: [10, 5]
        },
        {
          name: "SmallRightTriangle1Outline",
          x: 175,
          y: 50,
          offsetX: -25,
          offsetY: 50,
          rotation: 0,
          points: [0, 0, 0, 100, -50, 50],
          stroke: "red",
          strokeWidth: 2,
          closed: true,
          draggable: false,
          dash: [10, 5]
        },
        {
          name: "SmallRightTriangle2Outline",
          x: 100,
          y: 125,
          offsetX: 0,
          offsetY: 25,
          rotation: 0,
          points: [0, 0, 50, 50, -50, 50],
          stroke: "red",
          strokeWidth: 2,
          closed: true,
          draggable: false,
          dash: [10, 5]
        },
        {
          name: "Square1Outline",
          x: 150,
          y: 100,
          offsetX: 50,
          offsetY: 0,
          rotation: 0,
          points: [0, 0, 50, -50, 100, 0, 50, 50],
          stroke: "red",
          strokeWidth: 2,
          closed: true,
          draggable: false,
          dash: [10, 5]
        },
        {
          name: "Parallelogram1Outline",
          x: 75,
          y: 175,
          offsetX: 25,
          offsetY: 25,
          rotation: 0,
          points: [0, 0, 100, 0, 50, 50, -50, 50],
          stroke: "red",
          strokeWidth: 2,
          closed: true,
          draggable: false,
          dash: [10, 5]
        }
      ],
      pieces: [
        {
          name: "LargeRightTriangle1",
          x: 60,
          y: 240,
          offsetX: 50,
          offsetY: 100,
          rotation: 0,
          points: [0, 0, 0, 200, 100, 100],
          // stroke: "black",
          // strokeWidth: 3,
          fill: "orange",
          closed: true,
          draggable: true
        },
        {
          name: "LargeRightTriangle2",
          x: 140,
          y: 70,
          offsetX: 100,
          offsetY: 50,
          rotation: 0,
          points: [0, 0, 200, 0, 100, 100],
          // stroke: "black",
          // strokeWidth: 3,
          fill: "green",
          closed: true,
          draggable: true
        },
        {
          name: "MediumRightTriangle1",
          x: 105,
          y: 175,
          offsetX: -25,
          offsetY: -25,
          rotation: 0,
          points: [0, 0, 0, -100, -100, 0],
          fill: "red",
          closed: true,
          draggable: true
        },
        {
          name: "SmallRightTriangle1",
          x: 180,
          y: 100,
          offsetX: -25,
          offsetY: 50,
          rotation: 0,
          points: [0, 0, 0, 100, -50, 50],
          fill: "brown",
          closed: true,
          draggable: true
        },
        {
          name: "SmallRightTriangle2",
          x: 80,
          y: 100,
          offsetX: 0,
          offsetY: 25,
          rotation: 0,
          points: [0, 0, 50, 50, -50, 50],
          fill: "darkblue",
          closed: true,
          draggable: true
        },
        {
          name: "Square1",
          x: 250,
          y: 200,
          offsetX: 50,
          offsetY: 0,
          rotation: 0,
          points: [0, 0, 50, -50, 100, 0, 50, 50],
          fill: "yellow",
          closed: true,
          draggable: true
        },
        {
          name: "Parallelogram1",
          x: 175,
          y: 255,
          offsetX: 25,
          offsetY: 25,
          rotation: 0,
          points: [0, 0, 100, 0, 50, 50, -50, 50],
          fill: "lightblue",
          closed: true,
          draggable: true
        }
      ],
      solutionPaths: []
    }
  },
  computed: {
    score() {
      let score = 0;
      for(let i=0; i<this.outlines.length; i++) {
        if(this.pieces[i].x == this.outlines[i].x && this.pieces[i].y == this.outlines[i].y && this.pieces[i].rotation == this.outlines[i].rotation) {
          score++;
        }
      }
      return score;
    }
  },
  mounted() {
    this.stageConfig.width = document.getElementsByClassName('stage')[0].getBoundingClientRect().width;
    this.outlines.forEach(e => {
      e.x += this.stageConfig.width/2.3;
      e.y += this.stageConfig.height/3;
    })
  },
  methods: {
    isNearOutline(shape, outline) {
      var a = shape;
      var o = outline;
      var ax = a.x;
      var ay = a.y;

      if (ax > o.x - 20 && ax < o.x + 20 && ay > o.y - 20 && ay < o.y + 20 && a.rotation == o.rotation) {
        return true;
      } else {
        return false;
      }
    },
    roundToNearest(num, rountTo) {
      return Math.round(num / rountTo) * rountTo;
    },
    shiftItemToTail(arr, i) {
      return [...arr.filter((e, j) => i !== j), arr[i]];
    },
    setItemStateAndPopWithFunc(i, itemStateFunc) {
        this.pieces = this.shiftItemToTail(
          this.pieces.map((cp, j) => {
            return j === i ? { ...cp, ...itemStateFunc(cp) } : cp;
          }),
          i
        )
        this.outlines = this.shiftItemToTail(this.outlines, i);
    },
    onDragStart(e, i) {
      this.pieces = this.shiftItemToTail(this.pieces, i);
      this.outlines = this.shiftItemToTail(this.outlines, i);
    },
    onDragEnd(e, i) {
      this.setItemStateAndPopWithFunc(i, () => ({
        x: this.roundToNearest(e.target.attrs.x, this.snapToPer),
        y: this.roundToNearest(e.target.attrs.y, this.snapToPer)
      }));

      if(this.isNearOutline(this.pieces[i], this.outlines[i])) {
        this.pieces[i].x = this.outlines[i].x;
        this.pieces[i].y = this.outlines[i].y;
      }
    },
    onClick(e, i) {
      this.setItemStateAndPopWithFunc(i, cp => ({
        rotation: (cp.rotation + 45) % 360
      }));
    },
    onTap(e, i) {
      this.setItemStateAndPopWithFunc(i, cp => ({
        rotation: (cp.rotation + 45) % 360
      }));
    }
  }
}
</script>

<style>
#app {
  width: 100vw;
  min-height: 100vh;
}
.stage {
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  transition: all 200ms ease-out;
}
.success {
  background: rgb(52, 221, 52);
  box-shadow: 0 10px 20px rgba(52, 221, 52, 0.6);
}
</style>
