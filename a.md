 ### Vue风格指南
 #### 必要的

 1.  组件名为多个单词
 2.  组件的data必须是一个函数
 3.  prop尽量详细，至少需要指定其类型
   
    ```
     props: {
     status: {
    type: String,
    required: true,
    validator: function (value) {
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error'
      ].indexOf(value) !== -1
    }
    }
    }

    ```
 4.  使用v-for时必须设置key值,并且尽量不用index
 5.  避免v-if和v-for用在一起
   * 使用计算属性进行过滤
   * 将v-if移动到外部template标签上
6. 为组件样式设置作用域
7. 私有 property 名
   
   ```
   var myGreatMixin = {
   // ...
   methods: {
     $_myGreatMixin_update: function () {
      // ...
    }
   }
   }
   ```

#### 强烈推荐
1. 组件文件,每个组件单独分成文件
2. 单文件组件的文件名应该要么始终是单词大写开头 (PascalCase)，要么始终是横线连接 (kebab-case)。
3. 基础组件名
   应用特定样式和约定的基础组件 (也就是展示类的、无逻辑的或无状态的组件) 应该全部以一个特定的前缀开头，比如 Base、App 或 V。
4. 单例组件名
   只应该拥有单个活跃实例的组件应该以 The 前缀命名，以示其唯一性。

   这不意味着组件只可用于一个单页面，而是每个页面只使用一次。这些组件永远不接受任何 prop，因为它们是为你的应用定制的，而不是它们在你的应用中的上下文。如果你发现有必要添加 prop，那就表明这实际上是一个可复用的组件，只是目前在每个页面里只使用一次。
5. 紧密耦合的组件名
   和父组件紧密耦合的子组件应该以父组件名作为前缀命名。
   components/
       |- TodoList.vue
       |- TodoListItem.vue
       |- TodoListItemButton.vue
6. 组件名中的单词顺序强烈推荐
    组件名应该以高级别的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。
7. 自闭合组件
8. 模板中的组件名大小写
    在所有组件中使用<my-component></my-component>
9. JS/JSX中的组件名大小写
10. 完整单词的组件名
    组件名应该倾向于完整单词而不是缩写.
11. Prop名的大小写
    ```
        props: {
            greetingText: String
        }
        <WelcomeMessage greeting-text="hi"/>
    ```
12. 多个attribute的元素应该分行撰写,每个attribute一行.
13. 模板中简单的表达式
    组件模板应该只包含简单的表达式,复杂的表达式应该重构为计算属性
    ```
        <!-- 在模板中 -->
        {{ normalizedFullName }}

        // 复杂表达式已经移入一个计算属性
        computed: {
        normalizedFullName: function () {
            return this.fullName.split(' ').map(function (word) {
            return word[0].toUpperCase() + word.slice(1)
            }).join(' ')
        }
        }
    ```
14. 简单的计算属性
    将复杂的计算属性分割为尽可能多的简单的property.
    ```
        computed: {
            basePrice: function () {
                return this.manufactureCost / (1 - this.profitMargin)
            },
            discount: function () {
                return this.basePrice * (this.discountPercent || 0)
            },
            finalPrice: function () {
                return this.basePrice - this.discount
            }
        }

    ```
15. 带引号的attribute值 
    `<AppSidebar :style="{ width: sidebarWidth + 'px' }">`
16. 指令缩写
    用: 表示 v-bind:、用 @ 表示 v-on: 和用 # 表示 v-slot:
#### 推荐
1. 组件/实例的选项的顺序
2. 元素attribute的顺序
3. 组件/实例选项中的空行
4. 单文件组件的顶级元素的顺序
   <template></template>
   <script></script>
   <style></style>



### nuxt.js
1. 在nuxt中使用jsencrypt
   ```
        const JSEncrypt = require("jsencrypt");
        let publicKey = security.publicKey;
        var encrypt = new JSEncrypt.JSEncrypt();
        encrypt.setPublicKey(publicKey);
        captchaLogin({
            mobile: this.mobile,
            password: encrypt.encrypt(this.password)
        })
   ```
2. 在fetch使用多个异步请求
    ```
        async fetch({ store, params }) {
            // store.commit("article/SET_CURRENT_PAGE", 1);
            // await store.dispatch("main/actIndexFloor");
            // store.dispatch("main/actIndexCustomClass");
            // store.dispatch("main/actIndexRecommendPage", 1);
            const arr = [
            store.dispatch("main/actIndexCustomClass"), // 商品分类
            store.dispatch("main/actIndexRecommendPage", 1), // 商品推荐
            store.dispatch("main/actIndexFloor"), //楼层信息
            ];
            return Promise.all(arr)
        }
    ```
   
3. 在asyncData中使用多个异步请求
   ```
        async asyncData({ params, store }) {
            return Promise.allSettled([
            // store.dispatch("main/actIndexCustomClass"),
            IndexAdv({ advKey: "pcIndex" }), // 首页轮播图
            IndexNav(), // 首页导航栏
            ]).then((data) => {
            console.log(data);
            return {
                // IndexCustomClassData: store.state.main.IndexCustomClassData,
                IndexAdvInPage: data[0].value.data,
                IndexNavList: data[1].value.data,
            };
        });
   ```

   git强制覆盖本地仓库
   git fetch --all &&  git reset --hard origin/master && git pull


git查看所有分支
    git branch -a
# 2.查看当前使用分支(结果列表中前面标*号的表示当前使用分支)
    git branch

# 3.切换分支
    git checkout 分支名


### 旋转动画
   -  使用setInterval
        ```
            setInterval("render()",150) // 周期性进行执行渲染函数
            function render() {
                renderer.render(scene,camera);//执行渲染操作
                mesh.rotateY(0.01);//每次绕y轴旋转0.01弧度
            }
        ```
    - 使用requestAnimationFrame
        ```
            function render(){
                renderer.render(scene, camera);
                mesh.rotateY(0.01)
                mesh.rotateX(0.02)
                mesh.rotateZ(0.03)
                requestAnimationFrame(render)
            }
        ```
    - 均匀旋转(计算两次相差时间，然后用时间*速度得到总的旋转角度，使结果匀速)
        ```
            let T0 = new Date();//上次时间
            function render() {
                let T1 = new Date();//本次时间
                let t = T1-T0;//时间差
                T0 = T1;//把本次时间赋值给上次时间
                requestAnimationFrame(render);
                renderer.render(scene,camera);//执行渲染操作
                mesh.rotateY(0.001*t);//旋转角速度0.001弧度每毫秒
            }
            render();
        ```
### 鼠标操作三维场景
OrbitControls.js控件支持鼠标左中右键操作和键盘方向键操作，具体代码如下，使用下面的代码替换1.1节中renderer.render(scene,camera);即可。
    ```
        function render() {
            renderer.render(scene,camera);//执行渲染操作
        }
        render();
        var controls = new THREE.OrbitControls(camera,renderer.domElement);//创建控件对象
        controls.addEventListener('change', render);//监听鼠标、键盘事件
    ```
注意开发中不要同时使用requestAnimationFrame()或controls.addEventListener('change', render)调用同一个函数，这样会冲突。
### 增加辅助三维坐标系
    ```
        var axisHelper = new THREE.AxisHelper(250);
        scene.add(axisHelper);
    ```
### 增加几何体
SphereGeometry

    ```
        SphereGeometry(radius, widthSegments, heightSegments)
    ```

| 参数           | 含义     |
| -------------- | -------- |
| redius         | 半径     |
| widthSegments  | 球面精度 |
| heightSegments | 球面精度 |


    ```
        // 立方体网格模型
        var geometry1 = new THREE.BoxGeometry(100, 100, 100);
        var material1 = new THREE.MeshLambertMaterial({
        color: 0x0000ff
        }); //材质对象Material
        var mesh1 = new THREE.Mesh(geometry1, material1); //网格模型对象Mesh
        scene.add(mesh1); //网格模型添加到场景中

        // 球体网格模型
        var geometry2 = new THREE.SphereGeometry(60, 40, 40);
        var material2 = new THREE.MeshLambertMaterial({
        color: 0xff00ff
        });
        var mesh2 = new THREE.Mesh(geometry2, material2); //网格模型对象Mesh
        mesh2.translateY(120); //球体网格模型沿Y轴正方向平移120
        scene.add(mesh2);

        // 圆柱网格模型
        var geometry3 = new THREE.CylinderGeometry(50, 50, 100, 25);
        var material3 = new THREE.MeshLambertMaterial({
        color: 0xffff00
        });
        var mesh3 = new THREE.Mesh(geometry3, material3); //网格模型对象Mesh
        // mesh3.translateX(120); //球体网格模型沿Y轴正方向平移120
        mesh3.position.set(120,0,0);//设置mesh3模型对象的xyz坐标为120,0,0
        scene.add(mesh3); //
    ```
### 设置材质效果
    ```
        var sphereMaterial=new THREE.MeshLambertMaterial({
            color:0xff0000,
            opacity:0.7,
            transparent:true
        });//材质对象
        // 或者
        material.opacity = 0.5 ;
        material.transparent = true ;
    ```

-  材质常见属性
  
| 材质属性    | 简介                                       |
| ----------- | ------------------------------------------ |
| color       | 材质颜色，例如蓝色0x0000ff                 |
| wireframe   | 将几何图形渲染为线框。 默认值为false       |
| opacity     | 透明度设置，0表示完全透明，1表示完全不透明 |
| transparent | 是否开启透明，默认false                    |

- 漫反色与镜反射：MeshLambertMaterial()、MeshPhongMaterial()
| 属性      | 简介           |
| --------- | -------------- |
| specular  | 球体高光颜色   |
| shininess | 光照强度的系数 |

- 材质类型
| 材质类型             | 功能                                                           |
| -------------------- | -------------------------------------------------------------- |
| MeshBasicMaterial    | 基础网格材质，不受光照影响的材质                               |
| MeshLambertMaterial  | Lambert网格材质，与光照有反应，漫反射                          |
| MeshPhongMaterial    | 高光Phong材质,与光照有反应                                     |
| MeshStandardMaterial | PBR物理材质，相比较高光Phong材质可以更好的模拟金属、玻璃等效果 |

### 光照效果设置
- 常见光源类型
| 光源             | 简介              |
| ---------------- | ----------------- |
| AmbientLight     | 环境光            |
| PointLight       | 点光源            |
| DirectionalLight | 平行光,比如太阳光 |
| SpotLight        | 聚光源            |

- 点光源创建
```
    //点光源
    var point = new THREE.PointLight(0xffffff);
    point.position.set(400, 200, 300); //点光源位置
    // 通过add方法插入场景中，不插入的话，渲染的时候不会获取光源的信息进行光照计算
    scene.add(point); //点光源添加到场景中
```
- 环境光创建
    ```
        //环境光    环境光颜色与网格模型的颜色进行RGB进行乘法运算
        var ambient = new THREE.AmbientLight(0x444444);
        scene.add(ambient);
    ```
光源通过add方法插入场景中，不插入的话，渲染的时候不会获取光源的信息进行光照计算

- 光源位置
    `point.position.set(400, 200, 300); //点光源位置`
如果只设置一个点光源的情况下，你通过鼠标旋转操作整个三维场景，你会发现立方体点光源无法照射的地方相对其他位置会比较暗，你可以通过下面的代码在新的位置插入一个新的光源对象。点光源设置的位置是(-400, -200, -300)，相当于把立方体夹在两个点光源之间。

```
    // 点光源2  位置和point关于原点对称
    var point2 = new THREE.PointLight(0xffffff);
    point2.position.set(-400, -200, -300); //点光源位置
    scene.add(point2); //点光源添加到场景中
```