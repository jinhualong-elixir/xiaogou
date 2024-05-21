<template>
  <div class="page">
      <h2>有你的周六</h2>
      <p>这里好像没什么可以看的呀</p>
      <p>但是没关系，有你在我们去哪儿都很开心</p>
      <p>这样子躺在草地上也很浪漫不是？</p>
  </div>
</template>

<script>
export default {
  name: 'SlideSix',
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

</style>
