<template>
<div>
  
  <canvas
    ref="my-canvas"
    @resize="resize"
    @mousemove="onMouseMove"
    @touchmove="onTouchMove"
    @touchend="onMouseLeave"
    @mouseleave="onMouseLeave"
  ></canvas>
    <button @click="resize">This is wrapper</button>
</div>


</template>

<script>
export default {
  name: 'Canvas',

  data(){
    return {
      STAR_COUNT: 0,
      STAR_SIZE: 0,
      STAR_MIN_SCALE: 0,
      OVERFLOW_THRESHOLD: 0,

      scale: 1, // device pixel ratio
      width: 0,
      height: 0,
      stars: [],
      pointerX: 0,
      pointerY: 0,
      velocity: { x: 0, y: 0, tx: 0, ty: 0, z: 0.0005 },
      touchInput: false,
      canvas: null,
      context: null,
    };
  },

  mounted() {
    this.STAR_COUNT = ( this.$refs["my-canvas"].clientWidth + this.$refs["my-canvas"].clientHeight ) / 8,
    this.STAR_SIZE = 3,
    this.STAR_MIN_SCALE = 0.2,
    this.OVERFLOW_THRESHOLD = 50;
    this.canvas = document.querySelector( 'canvas' ),
    this.context = this.$refs['my-canvas'].getContext('2d'),
    this.generate();
    this.resize();
    this.step();
  },

  methods: {
    generate() {
      console.log(this.STAR_COUNT);
      console.log(this);
      console.log(this.$refs["my-canvas"].clientHeight);
      for( let i = 0; i < this.STAR_COUNT; i++ ) {
        this.stars.push({
          x: 0,
          y: 0,
          z: this.STAR_MIN_SCALE + Math.random() * ( 1 - this.STAR_MIN_SCALE )
        });
      }
    },
    placeStar( star ) {
      star.x = Math.random() * this.width;
      star.y = Math.random() * this.height;
    },
    recycleStar( star ) {
      let direction = 'z';
      let vx = Math.abs( this.velocity.x ),
          vy = Math.abs( this.velocity.y );
      if( vx > 1 || vy > 1 ) {
        let axis;
        if( vx > vy ) {
          axis = Math.random() < vx / ( vx + vy ) ? 'h' : 'v';
        }
        else {
          axis = Math.random() < vy / ( vx + vy ) ? 'v' : 'h';
        }
        if( axis === 'h' ) {
          direction = this.velocity.x > 0 ? 'l' : 'r';
        }
        else {
          direction = this.velocity.y > 0 ? 't' : 'b';
        }
      }

      star.z = this.STAR_MIN_SCALE + Math.random() * ( 1 - this.STAR_MIN_SCALE );
      if( direction === 'z' ) {
        star.z = 0.1;
        star.x = Math.random() * this.width;
        star.y = Math.random() * this.height;
      }
      else if( direction === 'l' ) {
        star.x = -this.OVERFLOW_THRESHOLD;
        star.y = this.height * Math.random();
      }
      else if( direction === 'r' ) {
        star.x = this.width + this.OVERFLOW_THRESHOLD;
        star.y = this.height * Math.random();
      }
      else if( direction === 't' ) {
        star.x = this.width * Math.random();
        star.y = -this.OVERFLOW_THRESHOLD;
      }
      else if( direction === 'b' ) {
        star.x = this.width * Math.random();
        star.y = this.height + this.OVERFLOW_THRESHOLD;
      }
    },
    resize() {
      // console.log('resize');

      this.scale = window.devicePixelRatio || 1;
      this.width = 500;
      // this.width = window.innerWidth * this.scale;
      this.height = 500;
      // this.height = window.innerHeight * this.scale;
      this.canvas.width = this.width;
      this.canvas.height = this.height;
      this.stars.forEach( this.placeStar );
    },
    step() {
        // console.log('step');
      this.context.clearRect( 0, 0, this.width, this.height );
      this.update();
      this.render();
      requestAnimationFrame( this.step );
    },
    update() {
      this.velocity.tx *= 0.96;
      this.velocity.ty *= 0.96;
      this.velocity.x += ( this.velocity.tx - this.velocity.x ) * 0.8;
      this.velocity.y += ( this.velocity.ty - this.velocity.y ) * 0.8;
      this.stars.forEach( ( star ) => {
        star.x += this.velocity.x * star.z;
        star.y += this.velocity.y * star.z;
        star.x += ( star.x - this.width/2 ) * this.velocity.z * star.z;
        star.y += ( star.y - this.height/2 ) * this.velocity.z * star.z;
        star.z += this.velocity.z;
        // recycle when out of bounds
        if( star.x < -this.OVERFLOW_THRESHOLD || star.x > this.width + this.OVERFLOW_THRESHOLD || star.y < -this.OVERFLOW_THRESHOLD || star.y > this.height + this.OVERFLOW_THRESHOLD ) {
          this.recycleStar( star );
        }
      });
    },
    render() {
// console.log('render', this.stars);

      this.stars.forEach( ( star ) => {
        this.context.beginPath();
        this.context.lineCap = 'round';
        this.context.lineWidth = this.STAR_SIZE * star.z * this.scale;
        this.context.strokeStyle = 'rgb(255,255,255)';
        // this.context.strokeStyle = 'rgba(255,255,255,'+(0.5 + 0.5*Math.random())+')';
        this.context.beginPath();
        this.context.moveTo( star.x, star.y );
        var tailX = this.velocity.x * 2,
            tailY = this.velocity.y * 2;
        // stroke() wont work on an invisible line
        if( Math.abs( tailX ) < 0.1 ) tailX = 0.5;
        if( Math.abs( tailY ) < 0.1 ) tailY = 0.5;
        this.context.lineTo( star.x + tailX, star.y + tailY );
        this.context.stroke();

// console.log(star);
      });
    },
    movePointer( x, y ) {
      console.log('mousemove  ', 'x: ',x, 'y: ', y);

      if( typeof pointerX === 'number' && typeof pointerY === 'number' ) {
        let ox = x - this.pointerX,
            oy = y - this.pointerY;
        this.velocity.tx = this.velocity.tx + ( ox / 8*this.scale ) * ( this.touchInput ? 1 : -1 );
        this.velocity.ty = this.velocity.ty + ( oy / 8*this.scale ) * ( this.touchInput ? 1 : -1 );
      }
      this.pointerX = x;
      this.pointerY = y;
    },
    onMouseMove( event ) {
      // console.log('mousemove  ', event);
      this.touchInput = false;
      this.movePointer( event.offsetX, event.offsetY );
    },
    onTouchMove( event ) {
      this.touchInput = true;
      this.movePointer( event.touches[0].clientX, event.touches[0].clientY, true );
      event.preventDefault();
    },
    onMouseLeave() {
      this.pointerX = null;
      this.pointerY = null;
    },
  },
}
</script>

<style scoped>
div {
  background-color: black;
  margin: 0 auto;

  width: 50vw;
  height: 50vh;
}
canvas {
  width: 100%;
  height: 90%;
}
</style>