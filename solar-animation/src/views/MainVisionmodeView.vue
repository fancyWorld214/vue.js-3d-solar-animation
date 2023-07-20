<template>
  <div>
    <button ref="btn_1">日食</button>
    <button ref="btn_2">月食</button>
    <button ref="ChangeButton" @click="changeskin">换肤</button>
    <canvas id="overallCanvas" ref="overallCanvas"></canvas>
    <canvas id="renderCanvas" ref="renderCanvas"></canvas>
    <canvas id="moonCanvas" ref="moonCanvas"></canvas>
    <video ref="video" autoplay preload="auto"></video>
    <video ref="video2" autoplay preload="auto"></video>
  </div>
</template>

<style>
/* 将原有的CSS样式复制到这里 */
canvas {
  width: 100%;
  height: 95%;
  padding: 0;
  margin: 0;
  overflow: hidden;
}

#renderCanvas {
  display: none;
}

#moonCanvas {
  display: none;
}

button {
  color: black;
  font-display: block;
}

video {
  display: none;
}
</style>

<script>
import * as BABYLON from "babylonjs";
import "./MainVision/hand.js";
//import Vue from 'vue';
import MainVision from "./MainVision/MainVision.js";
import SunEclipse from "./MainVision/SunEclipse.js";
import MoonEclipse from "./MainVision/MoonEclipse.js";

export default {
  name: "MainVisionmodeView",
  components: {
    // HelloWorld
  },
  mounted() {
    this.initial();
  },
  methods: {
      changeskin(){
        // 当按钮被点击时执行的操作
          this.mainvision.changeskin();
      },
      async initial() {
        if (!BABYLON.Engine.isSupported()) {
          window.alert("Browser not supported");
        } else {
          // 初始化场景要素
          var canvas0 = this.$refs.overallCanvas;
          var canvas1 = this.$refs.renderCanvas;
          var canvas2 = this.$refs.moonCanvas;
          const engine0 = new BABYLON.WebGPUEngine(canvas0);
          await engine0.initAsync();
          var scene0 = new BABYLON.Scene(engine0);
          const engine1 = new BABYLON.WebGPUEngine(canvas1);
          await engine1.initAsync();
          var scene1 = new BABYLON.Scene(engine1);
          const engine2 = new BABYLON.WebGPUEngine(canvas2);
          await engine2.initAsync();
          var scene2 = new BABYLON.Scene(engine2);

          // 添加点击事件监听器


          // 配置相机
          var camera1 = new BABYLON.ArcRotateCamera(
            "camera1",
            0,
            0,
            10,
            BABYLON.Vector3.Zero(),
            scene1
          );
          camera1.attachControl(canvas1, true);

          var camera2 = new BABYLON.ArcRotateCamera(
            "camera2",
            0,
            0,
            10,
            BABYLON.Vector3.Zero(),
            scene2
          );
          camera2.attachControl(canvas2, true);

          var camera0 = new BABYLON.ArcRotateCamera(
            "ArcRotateCamera",
            -Math.PI / 4.0,
            0.25 * Math.PI,
            1.5,
            new BABYLON.Vector3(0, 0, 0),
            scene0
          );
          // Attach the camera to the canvas
          camera0.attachControl(canvas0, true);

          // 配置场景
          var mainvision = new MainVision(canvas0, engine0, scene0, camera0);
          var suneclipse = new SunEclipse(canvas1, engine1, scene1, camera1);
          var mooneclipse = new MoonEclipse(canvas2, engine2, scene2, camera2);
          mainvision.Building();
          suneclipse.Building();
          mooneclipse.Building();

          // 将canvas转换为video
          const video = this.$refs.video;
          const video2 = this.$refs.video2;

          // Create a MediaStream object from the canvas
          const stream = canvas1.captureStream();
          const stream2 = canvas2.captureStream();

          // Create a new MediaRecorder instance for recording video
          const recorder = new MediaRecorder(stream);
          const recorder2 = new MediaRecorder(stream2);

          // Create an array to store the recorded video chunks
          var chunks = [];
          var chunks2 = [];

          // Listen for dataavailable event which is triggered when a video chunk is available
          recorder.addEventListener("dataavailable", (event) => {
            chunks.push(event.data);
          });
          recorder2.addEventListener("dataavailable", (event) => {
            chunks2.push(event.data);
          });

          // Listen for stop event which is triggered when recording has stopped
          recorder.addEventListener("stop", () => {});
          recorder2.addEventListener("stop", () => {});

          // Start recording the canvas
          recorder.start();
          recorder2.start();

          // Stop recording after 5 seconds (adjust this according to your needs)
          setInterval(() => {
            // Create a Blob object from the recorded chunks
            const blob = new Blob(chunks, { type: "video/webm" });
            recorder.stop();
            // Set the source of the video element to the recorded video
            const videoURL = URL.createObjectURL(blob);
            video.src = videoURL;
            chunks = [];
            recorder.start();
          }, 50);
          setInterval(() => {
            // Create a Blob object from the recorded chunks
            const blob = new Blob(chunks2, { type: "video/webm" });
            recorder2.stop();
            // Set the source of the video element to the recorded video
            const videoURL = URL.createObjectURL(blob);
            video2.src = videoURL;
            chunks2 = [];
            recorder2.start();
          }, 50);

          const btn_1 = this.$refs.btn_1;
          const btn_2 = this.$refs.btn_2;
          //必须要video的metadata加载完成后才能进入画中画
          video.addEventListener("loadedmetadata", () => {
            btn_1.addEventListener("click", () => {
              video
                .requestPictureInPicture()
                .then(() => {
                  // 成功进入画中画模式
                })
                .catch((error) => {
                  // 处理错误
                  console.error("Failed to enter picture-in-picture mode:", error);
                });
            });
          });
          video2.addEventListener("loadedmetadata", () => {
            btn_2.addEventListener("click", () => {
              video2
                .requestPictureInPicture()
                .then(() => {
                  // 成功进入画中画模式
                })
                .catch((error) => {
                  // 处理错误
                  console.error("Failed to enter picture-in-picture mode:", error);
                });
            });
          });

          // setUpCameraMovingPattern(camera0, scene0);

          // Resize
          window.addEventListener("resize", function () {
            engine0.resize();
          });


          this.mainvision = mainvision;
        }
      },
  }
};
</script>
