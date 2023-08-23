<template>
  <div class="three-container">
    <div id="webgl"></div>
  </div>
</template>
<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
export default {
  data() {
    return {
      scene: null, // 场景
      camera: null, // 相机
      renderer: null, // 渲染器
      mesh: null, // 模型
      geometry: null, // 形状
      material: null, // 材质
      controls: null, // 控制器
      modelContainer: null
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.init();
    });
    window.onresize = () => {
      // 重置渲染器输出画布canvas尺寸
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      // 全屏情况下：设置观察范围长宽比aspect为窗口宽高比
      this.camera.aspect = window.innerWidth / window.innerHeight;
      // 渲染器执行render方法的时候会读取相机对象的投影矩阵属性projectionMatrix
      // 但是不会每渲染一帧，就通过相机的属性计算投影矩阵(节约计算资源)
      // 如果相机的一些属性发生了变化，需要执行updateProjectionMatrix ()方法更新相机的投影矩阵
      this.camera.updateProjectionMatrix();
    };
  },
  methods: {
    init() {
      //第一步：创建场景、相机、渲染器
      this.scene = new THREE.Scene();
      this.initCameraBase() //渲染基础3d模型相机
      //第二步：创建要显示的立方体，设置它的属性材质，并把它放入场景里
      this.initMesh();


      // this.initCamera(); //渲染相机
      // this.loadGLTF(); //渲染模型
      // this.initBackground() // 设置背景
      this.initRender(); //渲染器
      //创建光源
      this.initLight(); //加载灯光
      
      //鼠标操作三维场景
      this.controls = new OrbitControls(this.camera, this.renderer.domElement); //创建控件对象

      //第三步：渲染场景
      this.animateBase() // 加载基础3d模型场景
      // this.animate(); //加载渲染场景


    },
    loadGLTF() {
      var loader = new GLTFLoader();
      console.log(loader);

      loader.load(
        "/static/雪山222.gltf",
        (gltf) => {
          var model = gltf.scene;
          this.modelContainer = new THREE.Object3D();
          this.modelContainer.add(model);
          this.scene.add(this.modelContainer);
        },
        undefined,
        function (error) {
          console.error(error);
        }
      );
    },
    animateBase() {
      // this.mesh.rotation.x += 0.01
      this.mesh.rotation.y += 0.01
      requestAnimationFrame(this.animateBase);
      this.renderer.render(this.scene, this.camera);
    },
    animate() {
      requestAnimationFrame(this.animate);
      if (this.modelContainer) {
        this.modelContainer.rotation.y += 0.01; // 控制旋转速度和方向
      }
      this.renderer.render(this.scene, this.camera);
    },
    initCamera() {
      this.camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        1,
        1000
      );
      this.camera.position.set(10, 10, 10);
      this.camera.lookAt(0, 0, 0); //指向this.mesh对应的位置
    },
    initCameraBase() {
      this.camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        1,
        1000
      );
      this.camera.position.set(350, 350, 350);
      this.camera.lookAt(0, 0, 0); //指向this.mesh对应的位置
    },
    initMesh() {
      this.geometry = new THREE.BoxGeometry(100, 100, 100); //立方体
      
      this.material = new THREE.MeshBasicMaterial({
        color: 0x888888
      });
      this.mesh = new THREE.Mesh(this.geometry, this.material);

      //设置网格模型在三维空间中的位置坐标，默认是坐标原点
      this.mesh.position.set(0, 0, 0);

      this.scene.add(this.mesh);

    },
    initBackground () {

       // 创建一个立方体纹理加载器
      const cubeTextureLoader = new THREE.CubeTextureLoader();

      // 加载六个图像
      const texture = cubeTextureLoader.load([
          '/static/px.png', // 右侧面
          '/static/nx.png', // 左侧面
          '/static/py.png', // 顶部面
          '/static/ny.png', // 底部面
          '/static/pz.png', // 前侧面
          '/static/nz.png'  // 后侧面
      ]);
      this.scene.background = texture
    },
    initLight() {
      //点光源：两个参数分别表示光源颜色和光照强度
      // 参数1：0xffffff是纯白光,表示光源颜色
      // 参数2：1.0,表示光照强度，可以根据需要调整
      const pointLight = new THREE.DirectionalLight(0xffffff, 1.0);
      const pointLights = new THREE.DirectionalLight(0xffffff, 2.0);
      const pointLightss = new THREE.DirectionalLight(0xffffff, 1.0);
      pointLight.position.set(100, 50, 50);
      pointLights.position.set(-50, 50, -50);
      pointLightss.position.set(50, 100, -50);
      this.scene.add(pointLight);
      this.scene.add(pointLights);
      this.scene.add(pointLightss);
      // 辅助三维坐标轴
      const axesHelper = new THREE.AxesHelper(200);
      this.scene.add(axesHelper);
    },
    initRender() {
      this.renderer = new THREE.WebGLRenderer();

      //设置渲染器背景颜色
      // this.renderer.setClearColor(0x888888);
      //设置渲染器像素分辨值
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      document.getElementById("webgl").appendChild(this.renderer.domElement);
    },
  },
};
</script>
<style scoped>
.three-container {
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
}
</style>
