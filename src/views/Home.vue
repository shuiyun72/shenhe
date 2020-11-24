<template>
  <div class="home_c">
    <div id="container"></div>
    <div class="body_content">
      <div class="header">
        <div class="header_c">
          <div class="logo">
            <img src="@assets/img/logo.png" alt="" class="logo_img" />
          </div>
          <div class="right">
            <div class="box">
              <div
                class="item"
                v-for="(item, index) in topNavList"
                :key="index"
              >
                {{ item.name }}
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="content">
        <div class="showpart1">
          <div class="item" v-for="(item,index) in centerList" :key="index">
            <img src="@assets/img/s7.png" class="item_img_c" alt="" />
            <div class="rongqi_bg">
              {{item.name}}
            </div>
          </div>
        </div>
      </div>
      <div class="footer">
        <div class="part1">
          <div class="call">
            <div class="name">联系电话：</div>
            <div class="val">18695970486</div>
          </div>
          <div class="call">
            <div class="name">QQ：</div>
            <div class="val">523266835</div>
          </div>
          <div class="call">
            <div class="name">邮箱：</div>
            <div class="val">523266835@qq.com</div>
          </div>
        </div>
        <div class="line"></div>
        <div class="part2">
          <div>Copyright © 2015 鸿迎网络 All Rights Reserved</div>
          <div>豫ICP备19005129号</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  components: {},
  data() {
    return {
      topNavList: [
        { name: "安卓" },
        { name: "苹果" },
        { name: "小程序" },
        { name: "BS端" },
      ],
      centerList: [
        { name: "关" },
        { name: "注" },
        { name: "请" },
        { name: "！" },
        { name: "敬" }
      ],
    };
  },
  methods:{
    
  },
  mounted() {
    var container = document.getElementById("container");
    var width = container.clientWidth;
    var height = container.clientHeight;
    var aspect = width / height;
    var renderer = new THREE.WebGLRenderer();
    renderer.setSize(width, height);
    container.appendChild(renderer.domElement);

    var scene = new THREE.Scene();

    var camera = new THREE.PerspectiveCamera(50, aspect, 0.1, 1000);
    camera.position.z = 500;

    let system = new THREE.Group(); // planetary system

    scene.add(new THREE.AmbientLight(0xffffff, 0.2));

    var light = new THREE.DirectionalLight(0xffffff, 2.5);
    light.position.set(1500, 2500, 0);
    scene.add(light);

    var material = new THREE.MeshLambertMaterial({
      color: 0x0c2d4d,
    });

    var planet = new THREE.Mesh(
      new THREE.IcosahedronGeometry(100, 3),
      material
    );

    for (var i = 0; i < planet.geometry.vertices.length; i++)
      planet.geometry.vertices[i].multiplyScalar(Math.random() * 0.05 + 0.95);

    planet.geometry.computeFlatVertexNormals();
    system.add(planet);

    var asteroids = new THREE.Group();

    for (var p = 0; p < Math.PI * 2; p = p + Math.random() * 0.15) {
      var asteroid = new THREE.Mesh(
        new THREE.IcosahedronGeometry(8, 0),
        material
      );

      var size = Math.random() * 0.5;
      for (var i = 0; i < asteroid.geometry.vertices.length; i++)
        asteroid.geometry.vertices[i].multiplyScalar(
          Math.random() * 0.5 + size
        );

      let rand = Math.random() * 60 - 30;
      asteroid.position.set(
        200 * Math.sin(p) + rand,
        rand,
        200 * Math.cos(p) + rand
      );

      asteroid.geometry.computeFlatVertexNormals();
      asteroids.add(asteroid);
    }

    system.add(asteroids);

    system.rotation.x = 0.1;
    system.rotation.y = -0.3;
    system.rotation.z = -0.4;

    scene.add(system);

    for (let i = 0; i < 10; i++) {
      let particles = new THREE.Points(
        new THREE.Geometry(),
        new THREE.PointsMaterial({
          size: Math.random() * 5,
        })
      );
      for (let j = 0; j < 20; j++) {
        var vertex = new THREE.Vector3();
        vertex.x = Math.random() * width * 1.1 - (width * 1.1) / 2;
        vertex.y = Math.random() * height * 1.1 - (height * 1.1) / 2;
        vertex.z = -500;
        particles.geometry.vertices.push(vertex);
        particles.material.color.setScalar(Math.random() * 0.4 + 0.2);
      }
      scene.add(particles);
    }

    function render() {
      requestAnimationFrame(render);

      planet.rotation.y += 0.001;
      planet.rotation.z -= 0.0005;

      asteroids.rotation.y += 0.003;

      renderer.render(scene, camera);
    }

    render();
  },
};
</script>
<style lang="scss" scoped>
.home_c {
  position: relative;
  .body_content {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    min-width: 680px;
    .rongqi {
      min-height: 100vh;
    }
    .content {
      min-height: calc(100vh - 287px);
      .showpart1 {
        width: 90%;
        min-width: 680px;
        margin: 0 auto;
        min-height: calc(100vh - 287px);
        position: relative;
        .item {
          position: absolute;
          width: 220px;
          height: 450px;
          top: 50%;
          margin-top: -225px;
          left: 50%;
          margin-left: -110px;
          &:hover {
            .rongqi_bg {
              transform:rotateY(360deg);
              background: rgba(0, 255, 0, 0.3);
            }
          }
          &:nth-child(1) {
            margin-left: calc(0 - 110px);
            z-index:2;
          }

          &:nth-child(2) {
            margin-left: calc(20% - 110px);
            z-index:1;
            transform: scale(.9);
          }
          &:nth-child(3) {
            margin-left: calc(-20% - 110px);
            z-index:1;
            transform: scale(.9);
          }
          &:nth-child(4) {
            margin-left: calc(40% - 110px);
            transform: scale(.8);
          }
          &:nth-child(5) {
            margin-left: calc(-40% - 110px);
            transform: scale(.8);
          }
          .rongqi_bg {
            background: rgba(255, 255, 255, 0.3);
            // border-radius: 60px;
            position: absolute;
            width: 189px;
            height: 338px;
            overflow: hidden;
            top: 54px;
            left: 13px;
            transition: all 1.5s;
            line-height: 338px;
            text-align: center;
          }
        }
        .item_img_c {
          width: 220px;
          height: auto;
        }
      }
    }
    .footer {
      display: flex;
      color: #fff;
      width: 100vw;
      min-width: 680px;
      justify-content: space-around;
      align-items: center;
      padding: 30px 0;
      background-color: rgba(255, 255, 255, 0.2);
      .part1 {
        display: inline-flex;
        flex-direction: column;
        justify-content: space-between;
        .call {
          display: inline-flex;
          & + .call {
            margin-top: 10px;
          }
          .name {
            width: 90px;
          }
        }
      }
      .line {
        width: 2px;
        background-color: #eee;
        height: 30px;
      }
      .part2 {
        display: inline-flex;
        flex-direction: column;
        justify-content: space-between;
        text-align: center;
        div + div {
          margin-top: 10px;
        }
      }
    }
    .header {
      padding: 30px 30px 10px;
      .header_c {
        display: flex;
        justify-content: space-between;
        .right {
          flex: 1;
          align-items: center;
          padding: 0 30px;
          display: inline-flex;
          .box {
            display: flex;
            justify-content: space-around;
            flex: 1;
            .item {
              // background: rgba(255,255,255,.2);
              padding: 10px 0;
              width: 15%;
              text-align: center;
              box-shadow: 0px 0px 10px #666 inset, 0px 0px 10px transparent;
              border-radius: 10px;
              transition: all 1.5s;
              &:hover {
                box-shadow: 0px 0px 10px #0f0 inset, 0px 0px 10px #ccc;
                cursor: pointer;
                color: #0f0;
              }
            }
          }
        }
      }
    }
    .logo_img {
      width: 100px;
      height: 100px;
    }
  }
}
</style>
