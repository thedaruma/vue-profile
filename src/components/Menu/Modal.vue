<template lang="html">
  <div class="overlay">
    <div class="panel">
<div class="panel-header">
This is a modal
</div>
<div class="panel-body">
This is the modal body
</div>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style lang="css">
.overlay{
  height:100vh;
  width:100vw;
  display:flex;
  position:fixed;
  top:0;
  left:0;
  z-index:999;
  align-items:center;
  justify-content:center;
  background-color:rgba(0,0,0,.7)
}
</style>
