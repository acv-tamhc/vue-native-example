<template>
  <view class="container">
    <camera class="container" :type="this.type"/>
  </view>
</template>

<style>
.container {
  flex: 1;
}
.text-color-primary {
  color: blue;
}
</style>

<script>
import { Camera, Permissions } from "expo";
export default {
 data: function() {
   return {
     hasCameraPermission: false,
     type: Camera.Constants.Type.back,
   };
 },
 mounted: function() {
   Permissions.askAsync(Permissions.CAMERA)
     .then(status => {
       hasCameraPermission = status.status == "granted" ? true : false;
     }).catch((err)=>{
        console.log(err);
     });
 },
 components: { Camera },
};
</script>
