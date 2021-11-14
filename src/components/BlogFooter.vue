/* 页脚组件 */

<template>
<div>
    <br>
    <br>
    <br>
    <div id="footer">
        <p>www.dusaiphoto.com</p>
    </div>
</div>
   
</template>

<script>
    export default {
        name: 'BlogFooter'
    }
</script>

<style scoped>
    #footer {
        position: fixed;
        left: 0;
        bottom: 0;
        height: 50px;
        width: 100%;
          background-color: rgba(255, 255, 255, 0.2);
  background-image: 
    radial-gradient(closest-side, rgb(255, 0, 0), rgba(235, 105, 78, 0)),
    radial-gradient(closest-side, rgb(200, 243, 11), rgba(243, 11, 164, 0)),
    radial-gradient(closest-side, rgba(254, 234, 131, 1), rgba(254, 234, 131, 0)),
    radial-gradient(closest-side, rgba(170, 142, 245, 1), rgba(170, 142, 245, 0)),
    radial-gradient(closest-side, rgba(248, 192, 147, 1), rgba(248, 192, 147, 0));
        text-align: center;
        font-weight: bold;
    }
</style>