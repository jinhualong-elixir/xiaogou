<template>
  <div class="hello grid-container">
    <meting-js
      server="netease"
      type="playlist"
      id="7263067958"
      fixed="true"
      autoplay="true"
      loop="all"
      order="list"
      preload="auto"
      list-folded="true"
      list-max-height="500px"
      lrc-type="1"
    >
    </meting-js>
    <div class="letter" ref="letter">
      <button @click="handleBackClick">
        <el-icon><CloseBold /></el-icon>
      </button>
      <div class="canvas-container">
        <!-- <div class="real-bg"></div> -->
        <canvas ref="canvas" @mousedown="startScratch" @touchstart="startScratch"
            @mousemove="scratch" @touchmove="scratch" @mouseup="stopScratch" @touchend="stopScratch">
        </canvas>
        <div class='canvas-bg'>
          <!-- 这里放置底部文字和图案 -->
          <p>中奖啦！！！</p>
          <!-- <img src="bottom_image.jpg" alt="底部图案"> -->
        </div>
        <el-button class='priceBtn' plain @click="centerDialogVisible = true" v-if="revealed">
          <el-icon><Present /></el-icon>兑换礼品
        </el-button>

        <el-dialog
          v-model="centerDialogVisible"
          title="耶!"
          width="500"
  
        >
        <img src="@/assets/img/jellycat.png" alt="Your Image">
          <h3 style="text-align: center;">JellyCat一只任选</h3>
          
          <p style="text-align: center;">兑换方式: 加V 17373437778</p>
        </el-dialog>

        <!-- <el-button class= 'priceBtn' v-if="revealed" icon='el-icon-trophy' @click="redeemGift" type="info">兑换礼品</el-button> -->
    </div>
      </div>

    <div class="envelope-container" ref="envelope">
      <div class="envelope"></div>
      <div class="card" id="test" @click="handleCardClick">
        <h1 class="message">你是小狗欸</h1>
      </div>
      <div class="cover"></div>
      <div class="lid"></div>
    </div>
    <div
      v-for="i in 4"
      :key="i"
      :style="{ top: '0px', left: (i - 1) * 20 + '%' }"
      class="image-container"
    >
      <img :src="require(`@/assets/img/egg.png`)" class="image" />
    </div>
    <div
      v-for="i in 7"
      :key="i"
      :style="{ bottom: '35px', left: (i - 1) * (100 / 7) + '%' }"
      class="image-container"
    >
      <img
        :src="require(`@/assets/img/heart.png`)"
        class="image heart content"
        @click="handleHeartClick"
      />
    </div>
    <el-image
      class="bg"
      :src="require(`@/assets/img/p${currentIndex + 1}.jpg`)"
      fit="fill"
    ></el-image>
    <header>
      <div class="mirror">
        <video ref="video" autoplay></video>
      </div>
    </header>
    <main>
      <div class="carl">
        <el-carousel
          ref="carl"
          @change="carouselChange"
          :interval="7000000"
          card-width="700"
          type="card"
          height="500px"
        >
          <el-carousel-item>
            <i class="love fa fa-heart" @click="markHeart($event, 1)"></i>
            <SlideOne />
          </el-carousel-item>
          <el-carousel-item>
            <i class="love fa fa-heart" @click="markHeart($event, 2)"></i>
            <SlideTwo />
          </el-carousel-item>
          <el-carousel-item>
            <i class="love fa fa-heart" @click="markHeart($event, 3)"></i>
            <SlideThree />
          </el-carousel-item>
          <el-carousel-item>
            <i class="love fa fa-heart" @click="markHeart($event, 4)"></i>
            <SlideFour />
          </el-carousel-item>
          <el-carousel-item>
            <i class="love fa fa-heart" @click="markHeart($event, 5)"></i>
            <SlideFive />
          </el-carousel-item>
          <el-carousel-item>
            <i class="love fa fa-heart" @click="markHeart($event, 6)"></i>
            <SlideSix />
          </el-carousel-item>
          <el-carousel-item>
            <i class="love fa fa-heart" @click="markHeart($event, 7)"></i>
            <SlideSeven />
          </el-carousel-item>
        </el-carousel>
      </div>
    </main>

    <footer>
      Designed by Bryan Long•
      <a
        href="https://github.com/jinhualong-elixir/jinhualong-elixir.github.io"
        target="_blank"
        rel="noopener"
        >Github</a
      >
    </footer>
  </div>
</template>

<script>
import SlideOne from "./slides/SlideOne.vue";
import SlideTwo from "./slides/SlideTwo.vue";
import SlideThree from "./slides/SlideThree.vue";
import SlideFour from "./slides/SlideFour.vue";
import SlideFive from "./slides/SlideFive.vue";
import SlideSix from "./slides/SlideSix.vue";
import SlideSeven from "./slides/SlideSeven.vue";
export default {
  name: "HelloWorld",
  data() {
    return {
      currentIndex: 0,
      numIcons: 8,
      radius: 150,
      title: "一封情书",

      canvasContext: null,
      lastX: 0,
      lastY: 0,
      revealed: false,
      centerDialogVisible: false,
   };
  },
  components: {
    SlideOne,
    SlideTwo,
    SlideThree,
    SlideFour,
    SlideFive,
    SlideSix,
    SlideSeven,
  },
  props: {
    msg: String,
  },
  created() {
    navigator.mediaDevices
      .getUserMedia({ video: { facingMode: "user" } })
      .then((stream) => {
        this.$refs.video.srcObject = stream;
      })
      .catch((error) => {
        console.log(error);
      });
  },
  mounted() {
    this.canvasContext = this.$refs.canvas.getContext('2d');
    this.canvasContext.fillStyle = 'gray';
    this.canvasContext.fillRect(0, 0, this.$refs.canvas.width, this.$refs.canvas.height);
    this.canvasContext.font = '20px Arial';
    this.canvasContext.fillStyle = 'white';
    this.canvasContext.fillText('刮一刮', 50, 50);
    this.canvasContext.globalCompositeOperation = 'destination-out'; // 设置混合模式为destination-out，表示刮去Canvas上的内容
    this.canvasContext.lineWidth = 20; // 设置笔触的宽度为20像素
  },
  beforeUnmount() {
    const carl = this.$refs.carl.$el;
    carl.removeEventListener("wheel", this.handleWheel);
  },
  methods: {
    startScratch(e) {
      console.log('asdasdasd')
      debugger
      e.preventDefault();
      this.$refs.canvas.style.cursor = 'pointer';
      this.lastX = e.clientX || e.touches[0].clientX;
      this.lastY = e.clientY || e.touches[0].clientY;
      //this.canvasContext.globalCompositeOperation = 'destination-out';
      this.canvasContext.beginPath();
    },
    scratch(e) {
      e.preventDefault();
      if (!this.lastX || !this.lastY) {
        return;
      }
      const x = e.clientX || e.touches[0].clientX;
      const y = e.clientY || e.touches[0].clientY;
      const offsetX = this.$refs.canvas.offsetLeft+250;
      const offsetY = this.$refs.canvas.offsetTop+100;
     // console.log(this.lastX - offsetX, this.lastY - offsetY,x - offsetX,y - offsetY)

      this.canvasContext.moveTo(this.lastX - offsetX, this.lastY - offsetY);
      this.canvasContext.lineTo(x - offsetX, y - offsetY+10);
      this.canvasContext.stroke();
      this.lastX = x;
      this.lastY = y;
      this.checkRevealed();
    },
    stopScratch() {
      this.lastX = 0;
      this.lastY = 0;
      this.canvasContext.closePath();
      this.$refs.canvas.style.cursor = 'default';
      //this.checkRevealed();
    },
    checkRevealed() {
      const imageData = this.canvasContext.getImageData(0, 0, this.$refs.canvas.width, this.$refs.canvas.height);
      const pixels = imageData.data;
      let revealedPixels = 0;
      for (let i = 0; i < pixels.length; i += 4) {
        if (pixels[i + 3] === 0) {
          revealedPixels++;
        }
      }
      const totalPixels = this.$refs.canvas.width * this.$refs.canvas.height;
      const revealedPercentage = (revealedPixels / totalPixels) * 100;
      console.log(revealedPercentage)
      this.revealed = revealedPercentage >= 60; // 当刮开的百分比大于等于60%时，显示按钮
    },
    redeemGift() {
      alert('恭喜你，获得了一个奖品！');
    },
    handleHeartClick(e) {
      if (e.target.classList.contains("special")) {
        this.$refs.envelope.classList.toggle("actived");
      }
    },
    handleBackClick() {
      this.$refs.letter.classList.toggle("expanded");
      this.$refs.envelope.classList.toggle("actived");
    },
    handleCardClick() {
      this.$refs.letter.classList.toggle("expanded");
    },
    carouselChange(index) {
      this.currentIndex = index;
    },
    handleWheel(e) {
      const delta = e.deltaY;
      const carl = this.$refs.carl;
      if (delta > 0) {
        carl.next();
      } else {
        carl.prev();
      }
    },
    markHeart(e, i) {
      e.target.classList.toggle("active");

      let heartList = document.getElementsByClassName("heart");
      if (e.target.classList.contains("active")) {
        heartList[i - 1].classList.add("actived");
      } else {
        heartList[i - 1].classList.remove("actived");
      }

      if (document.getElementsByClassName("actived").length == 7) {
        heartList[heartList.length - 1].classList.add("special");
      } else {
        heartList[heartList.length - 1].classList.remove("special");
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.fla{
 margin: 50px;
  float: right;
  width: 140px;
  height: 206px;
  background-image: url("../assets/img/flamingo.png");
  background-size: cover;
}
h1,
p {
  font-family: yinhexi, Arial, Helvetica, sans-serif;
}
.el-icon {
  font-size: 40px;
}
@keyframes open {
  100% {
    transform: rotatex(180deg);
  }
}
/*信件合上的动画*/
@keyframes open-rev {
  from {
    transform: rotatex(-180deg);
  }
}
.card {
  position: absolute;
  /*白色的卡片*/
  background: white;
  height: 25vmin;
  width: 43vmin;
  display: flex;
  /*设置div水平显示*/
  flex-direction: column;
  /*设置flex子项在每个flex行的交叉轴上的中心对齐，交叉轴方向为column，即垂直方向**/
  align-items: center;
  /*设置flex子项在主轴上的中心对齐*/
  justify-content: center;
  left: 2.5vmin;
  top: 0vmin;
  cursor: pointer;
  animation: slide-rev 0.2s ease-out;

  -webkit-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3),
    0 0 40px rgba(0, 0, 0, 0.1) inset;
  -moz-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3),
    0 0 40px rgba(0, 0, 0, 0.1) inset;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3), 0 0 40px rgba(0, 0, 0, 0.1) inset;
  -o-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3), 0 0 40px rgba(0, 0, 0, 0.1) inset;
}
.letter {
  position: absolute;
  top: 0;
  left: 0;
  margin: 53px;
  height: 80%;
  width: 90%;
  opacity: 0;
  background: white;
  transition: all 0.8s ease-in-out;
  button {
    position: absolute;
    top: 20px;
    left: 20px;
    z-index: 1;
  }
  h1,
  p {
    color: black;
    display: none;
  }
}

.letter.expanded {
  z-index: 999;
  opacity: 1;
  -webkit-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3),
    0 0 40px rgba(0, 0, 0, 0.1) inset;
  -moz-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3),
    0 0 40px rgba(0, 0, 0, 0.1) inset;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3), 0 0 40px rgba(0, 0, 0, 0.1) inset;
  -o-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3), 0 0 40px rgba(0, 0, 0, 0.1) inset;

  h1,
  p {
    display: block;
  }
}

.paper {
  width: 100%;
  height: 100%;
  background-color: #fff;
  border: 1px solid #ccc;
  overflow-y: scroll;
  :nth-child(3){
    text-align: left;
  }
  :last-child{
    text-align: right;
  }
}
.canvas-container{
  display: flex;
    height: 100%;
    align-items: center;
    justify-content: center;
    background-image: url("../assets/img/canvas-bg.jpg");
      background-size: contain;
    canvas {
        position: absolute;
        border: 1px solid black;
        left: 50%;
        top: 50%;
        width: 500px;
        height: 200px;
        margin-left: -250px;
        margin-top: -100px;
    }
    .canvas-bg{
        position: absolute;
        left: 50%;
        top: 50%;
        width: 500px;
        height: 200px;
        margin-left: -250px;
        margin-top: -100px;
        background: white;
        p{
          line-height: 200px;
          text-align: center;
          font-size: 2rem;
        }
    }
    .priceBtn {
      display: block;
        left: 50%;
        top: 70%;
        width: 200px;
        height: 100px;
        margin-left: -100px;
        margin-top: -50px;
    }
}


.title {
  font-size: 40px;
  font-weight: bold;
  margin-bottom: 20px;
  padding: 50px 0px 50px 150px;
}

.l-content {
  font-size: 20px;
  line-height: 1.5;
  text-align: center;
  padding: 50px 150px 50px 150px;
}
.paper:before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: url("https://dogefs.s3.ladydaily.com/~/source/unsplash/photo-1586074299757-dc655f18518c?ixlib=rb-4.0.3&q=85&fmt=jpg&crop=entropy&cs=srgb&dl=marjan-blan-marjanblan-794QUz5-cso-unsplash.jpg");
  opacity: 0.3;
}

/*卡片从右侧滑入*/
@keyframes slideL {
  100% {
    transform: translatex(-100vmin);
  }
}
@keyframes slideL-rev {
  from {
    transform: translatex(-100vmin);
  }
}
.envelope-container.actived {
  /*卡片划出动画*/
  animation: slideL .8s;
  animation-delay: 0.3s;
  /*把卡面动画地从一个地方移动到另一个地方，并让它停留在那里。*/
  animation-fill-mode: forwards;
}
.envelope-container:not(.actived) {
  /*卡片划出动画*/
  animation: slideL-rev 1.0s;
  animation-fill-mode: forwards;
}
.envelope-container:hover .card {
  /*卡片划出动画*/
  animation: slide 0.2s;
  animation-delay: 0.5s;
  /*把卡面动画地从一个地方移动到另一个地方，并让它停留在那里。*/
  animation-fill-mode: forwards;
}

/*卡片划出信封*/
@keyframes slide {
  100% {
    transform: translatey(-15vmin);
    z-index: 2;
  }
}
/*卡片落回信封*/
@keyframes slide-rev {
  from {
    transform: translatey(-15vmin);
  }
}
.lid {
  position: absolute;
  height: 0;
  width: 0;

  border-top: 15vmin solid #ff8896;
  border-left: 24vmin solid transparent;
  border-right: 24vmin solid transparent;

  top: 0;
  /*设置旋转元素的基点位置，为盒子的顶部边缘*/
  transform-origin: top;
  animation: open-rev 2s;
}

.envelope-container:hover .lid {
  animation: open 0.5s;
  animation-fill-mode: forwards;
}
.cover {
  position: absolute;
  height: 0;
  width: 0;

  border-bottom: 15vmin solid #f5b5bb;
  border-left: 24vmin solid transparent;
  border-right: 24vmin solid transparent;
  top: 15vmin;
  z-index: 3;
}

.cover::after {
  /*left triangle*/
  position: absolute;
  content: "";
  border-left: 24.5vmin solid #ffbbc1;
  border-bottom: 15vmin solid transparent;
  border-top: 15vmin solid transparent;
  top: -15vmin;
  left: -24vmin;
}

.cover::before {
  position: absolute;
  content: "";
  border-right: 24.5vmin solid #ffbbc1;
  border-bottom: 15vmin solid transparent;
  border-top: 15vmin solid transparent;
  top: -15vmin;
  left: -0.5vmin;
}
.envelope-container {
  position: absolute;
  top: 50%;
  left: 120%;
  margin: -15vmin 0 0 -24vmin;
  z-index: 998;

  -webkit-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3),
    0 0 40px rgba(0, 0, 0, 0.1) inset;
  -moz-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3),
    0 0 40px rgba(0, 0, 0, 0.1) inset;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3), 0 0 40px rgba(0, 0, 0, 0.1) inset;
  -o-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3), 0 0 40px rgba(0, 0, 0, 0.1) inset;
}
.envelope-container::after{
  content: "";
  z-index: 998;
  position: absolute;
  right: -247px;
  top: -66px;
  display: block;
  width: 80%;
  height: 200%;
  background-image: url("../assets/img/myHandT.png");
  background-size: cover;
  transform: rotate(270deg) scaleX(1);
} 
/*相对定位，并设置背景色和大小*/
.envelope {
  position: relative;
  background: #eb7885;
  height: 30vmin;
  width: 48vmin;
}
.love {
  font-size: 2.5rem;
  position: absolute;
  top: 30px;
  right: 30px;
}
// .love:hover{
//   color: red;
// }
.love.active {
  color: red;
}
.heart {
  filter: grayscale(1);
}
.heart.actived {
  filter: unset;
}

.image-container {
  position: absolute;
  width: 13%;
  height: 13%;
  transform: translate(-50%, -50%);
  display: flex;
  justify-content: center;
  align-items: center;

  z-index: 3;
  margin: 45px 35px -35px 90px;
}
.image {
  max-width: 100%;
  max-height: 100%;
}

.hello {
  height: 100%;
}
.grid-container {
  display: grid;
  grid-template-rows: 100px 1fr 30px;
  grid-template-areas:
    "header"
    "main"
    "footer";
}

.mirror {
  height: 100%;
  width: 80px;
  float: right;
  border-radius: 50%;
  overflow: hidden;

  margin-right: 80px;

  -webkit-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3),
    0 0 40px rgba(0, 0, 0, 0.1) inset;
  -moz-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3),
    0 0 40px rgba(0, 0, 0, 0.1) inset;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3), 0 0 40px rgba(0, 0, 0, 0.1) inset;
  -o-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3), 0 0 40px rgba(0, 0, 0, 0.1) inset;
}
.mirror::after {
  content: "";
  position: absolute;
  height: 80px;
  width: 80px;
  background-image: url("../assets/img/demon.png");
  background-size: cover;
  background-position: center;
  right: 80px;
}
.carl {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

video {
  height: 100%;
  width: 100%;
  object-fit: cover;

  transform: scaleX(-1);
}

.el-carousel__item {
  width: 70%;
  left: -10%;
  display: flex;
  flex-direction: column;
  justify-content: center;

  -webkit-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3),
    0 0 40px rgba(0, 0, 0, 0.1) inset;
  -moz-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3),
    0 0 40px rgba(0, 0, 0, 0.1) inset;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3), 0 0 40px rgba(0, 0, 0, 0.1) inset;
  -o-box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3), 0 0 40px rgba(0, 0, 0, 0.1) inset;
}

.el-carousel__item:hover {
  .page::after {
    opacity: 0.55;
    z-index: -1;
  }
  .page {
    opacity: 1;
    background: transparent;
  }
}

.el-carousel__item:nth-child(3) {
  background-image: url("../assets/img/p1.jpg");
  background-size: cover;
  background-position: center;
}

.el-carousel__item:nth-child(4) {
  background-image: url("../assets/img/p2.jpg");
  background-size: cover;
  background-position: center;
}

.el-carousel__item:nth-child(5) {
  background-image: url("../assets/img/p3.jpg");
  background-size: cover;
  background-position: center;
}

.el-carousel__item:nth-child(6) {
  background-image: url("../assets/img/p4.jpg");
  background-size: cover;
  background-position: center;
}

.el-carousel__item:nth-child(7) {
  background-image: url("../assets/img/p5.jpg");
  background-size: cover;
  background-position: center;
}

.el-carousel__item:nth-child(8) {
  background-image: url("../assets/img/p6.jpg");
  background-size: cover;
  background-position: center;
}

.el-carousel__item:nth-child(9) {
  background-image: url("../assets/img/p7.jpg");
  background-size: cover;
  background-position: center;
}

header {
  grid-area: header;
  z-index: 1;

  padding: 10px;
}
main {
  grid-area: main;
}
footer {
  grid-area: footer;
  background: black;
  z-index: 1;
  text-align: end;
  line-height: 30px;
  a {
    color:#a2a1a1;
    margin-right: 20px;
  }
}

.bg {
  position: absolute;
  top: 0;
  object-fit: fill;
  left: 0;
  width: 100%;
  height: 100%;

  filter: blur(40px);
}
.bg::after {
  content: "";
  display: block;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1;
}

@keyframes zy {
  10% {
    transform: rotate(15deg);
  }
  20% {
    transform: rotate(-10deg);
  }
  30% {
    transform: rotate(5deg);
  }
  40% {
    transform: rotate(-5deg);
  }
  50%,
  100% {
    transform: rotate(0deg);
  }
}
/* 使用 */
.content.actived {
  animation: zy 2.5s 0.15s linear infinite;
  animation: zy 2.5s 0.15s linear infinite;
  animation: zy 2.5s 0.15s linear infinite;
  animation: zy 2.5s 0.15s linear infinite;
}

.content:hover {
  animation: unset;
  transform: scale(1.05);
}

.special {
  animation: zy 0.5s 0.05s linear infinite !important;
  animation: zy 0.5s 0.05s linear infinite !important;
  animation: zy 0.5s 0.05s linear infinite !important;
  animation: zy 0.5s 0.05s linear infinite !important;
}
.special:hover {
  animation: unset !important;
  transform: scale(1.05);
}
.special:active {
  transform: scale(1);
}

// /* 兼容写法 */
// animation: zy 2.5s .15s linear infinite;
// -moz-animation: zy 2.5s .15s linear infinite; /* Firefox */
// -webkit-animation: zy 2.5s .15s linear infinite; /* Safari and Chrome */
// -o-animation: zy 2.5s .15s linear infinite; /* Opera */

.card h1 {
  color: #000000;
}

.card:hover {
}

canvas {
  border: 1px solid black;
  z-index: 9999;
}
.priceBtn {
  display: block;
  margin-top: 10px;
}
</style>
