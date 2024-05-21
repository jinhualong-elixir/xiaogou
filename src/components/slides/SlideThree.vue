<template>
  <div class="page">
      <h2>想你的周三</h2>
      <p>小狗，每次看到桃花就会想起你的脸颊，你真美</p>
      <p>才没有油</p>
      <img :src="require(`@/assets/img/poppy1.jpg`)" class="image" />
  </div>
</template>

<script>
export default {
  name: 'SlideThree',
  data() {
    return {
      url: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg'
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,h2,h3,h4,h5,h6{
  font-family: yinhexi, Arial, Helvetica, sans-serif;
  font-weight: bold;
      color: darkgreen;
    font-size: 2.3rem;
}
p{
  font-family: yinhexi, Verdana, sans-serif;
  font-size: 25px;
  line-height: 1.5;
  color: #333333;
}

.page{
    background: white;
    height: 50%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    transition-property: all;
    transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
    transition-duration: .8s;
    opacity: 0;
}
.page::after{
    content: '';
    position: absolute;
    height: 50%;
    width: 100%;
    background: white;
    opacity: 0;
}

.image{
  transform: rotate(18deg);
  width: 18%;
  position: absolute;
  /* transition: ro; */
  right: 80%;
  top: 8%;
  opacity: .8;
}

</style>
