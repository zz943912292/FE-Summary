# FE-Summary

## JS
1. var、let和const  
For循环中var和let：var同一个引用不会变，let每次会生成新变量会变  
var和function会挂载到window，let、const和class不会  

2. 类型查询(数组)
typeof              typeof null   === 'object'  
instanceof  
.constructor   返回构造函数  
Object.getPrototypeOf()    返回原型  
Object.prototype.toString.call()  返回[Object **]  
Array.prototype.isPrototypeOf()   返回布尔  
Array.isArray()   返回布尔  

3. 浅拷贝和深拷贝:  
浅拷贝拷贝基本类型的值和引用类型的地址，Object.assing, Array.slice, Array.concat  
深拷贝拷贝开辟新的内存地址用于存放新对象

4. 时间戳，Date和moment.js  
```javascript
  new Date()  //时间
  Date.parse() // 时间戳
  moment.valueOf() // 获取时间戳
  moment.format('格式化方式') // 格式化时间
  moment.substract(), moment.add() // 时间加减
  moment.diff() // 时间比较
  moment.isBefore(), moment.isAfter() // 判断，返回Boolean
```



## HTML
1. DTD:文档类型定义  
混杂模式和标准模式，标准模式又分标准模式和准标准模式  
主要影响CSS的呈现，某些情况影像JS的解析执行  
Html5标准模式 <!DOCTYPE html>  

2. TCP和UDP  

|  | TCP | UDP |  
| ---- | ---- | ---- |  
| 是否连接 | 面向连接 | 无连接 |  
| 是否可靠 | 可靠传输， 流量控制和拥塞控制 | 不可靠传输 |  
| 连接对象个数 | 一对一 | 一对一，一对多，多对多 |  
| 传输方式 | 面向字节流 | 面向报文 |  
| 首部开销 | 20-60字节 | 8字节 |  
| 使用场景 | 实时应用（IP电话，视频会议，直播等） | 要求可靠传输的应用（文件传输等）|  

## CSS
1. Css优先级 https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity  
选择器：ID > 类(类，属性，伪类)  > 类型(标签，伪元素)  
直接样式>继承样式  

2. 清除浮动  
容器的高度不能自动伸长以适应内容的高度，使得内容溢出到容器外面而影响（甚至破坏）布局的现象。  
overflow：hidden, auto 构成BFC清除浮动  
原因：BFC的区域不会与box重叠， 独立布局环境  
创建BFC方法：float;  position:fixed, absolute; overflow:hidden, auto; display:inline-block, flex  

## React
1. react setState什么时候同步什么时候异步，为什么？  
由React控制的事件处理程序，以及生命周期函数调用setState不会同步更新state 。  
componentDidUpdate 或者 setState 的回调函数（setState(updater, callback)），这两种方式都可以保证在应用更新后触发。
***React控制之外的事件中调用setState是同步更新的。比如原生js绑定的事件，setTimeout/setInterval等。***  

2. hook使用规则：  
只能在函数最外层调用，不能在循环、条件和子函数中使用。  
只能在React的函数组件中使用。  

3. 自定义hook：自定义调用hook的方法，提取复用逻辑到函数中  
命名必须以use开头。  
不同的自定义hook之间不会共享State。  

4. Redux为什么要dispatch  
因为State是只读的，不能直接修改，需要传递action表明修改意图，然后集中处理这些意图。  

5. React事件处理使用箭头函数，如果作为子组件嵌套，跟随父组件更新每次创建一个新的函数影响性能。  
使用***class fields***方法可以避免这类性能问题。  




## 其他

### 高德地图 TODO:补充完整demo
#### API
1. 创建地图  
```javascript
  var map = new AMap.Map('container',{
    zoom: 10,  //设置地图显示的缩放级别
    center: [116.397428, 39.90923],//设置地图中心点坐标
    layers: [new AMap.TileLayer.Satellite()],  //设置图层,可设置成包含一个或多个图层的数组
    mapStyle: 'amap://styles/whitesmoke',  //设置地图的显示样式
    viewMode: '2D',  //设置地图模式
    lang:'zh_cn',  //设置地图语言类型
  });
```

2. 地图的生命周期  
```javascript
  // 地图加载完成
  map.on('complete', function(){
    // 地图图块加载完成后触发
  }); 
  // 地图销毁
  map.destroy();
```

3. 地图状态  
```javascript 
  map.getZoom() // 获取缩放级别
  map.getCenter() // 获取中心点
  map.setZoomAndCenter()  // 设置缩放级别和中心点
```

4. 覆盖物  
> 添加覆盖物  
```javascript 
  // 构造点标记
  var marker = new AMap.Marker({
      icon: "https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png",
      position: [116.405467, 39.907761]
  });
  // 构造矢量圆形
  var circle = new AMap.Circle({
      center: new AMap.LngLat("116.403322", "39.920255"), // 圆心位置
      radius: 1000,  //半径
      strokeColor: "#F33",  //线颜色
      strokeOpacity: 1,  //线透明度
      strokeWeight: 3,  //线粗细度
      fillColor: "#ee2200",  //填充颜色
      fillOpacity: 0.35 //填充透明度
  });
  // 添加覆盖物到地图上
  map.add([marker,circle]);
```
> 点标记标注  
marker.setLabel({content,offset,direction})  
direction为标注方向  
offset为标注偏移量，根据icon尺寸设置  
content为文本标注内容，一般使用HTML元素  
注：清空文本标注时，设置空HTML元素，否则可能导致点标记事件bug。
```javascript 
  content = '<div></div>';
```

5. 事件  
对map和覆盖物都可以使用on,off成员实现事件绑定和移除  
on( eventName, handler, context)  
off( eventName, handler, context)  


### 图表地图绘制

准备：
1. 可视化库：Antv.L7(React支持更好)或Echarts  
2. geoJson：地理交互数据，用于分隔区域  
  世界地图和全国地图都很容易下载  
  可从阿里云获取详细的全国地图，精确到区县 http://datav.aliyun.com/tools/atlas  
3. 地图底图：可通过高德地图或MapBox加载
  也可设置map的属性style='blank'不使用底图，离线加载

React+Antv.L7的具体代码：  
需要一个空的div渲染地图，设置宽高  
```javascript
import { AMapScene, LineLayer, MapboxScene, PolygonLayer } from '@antv/l7-react';
import * as React from 'react';
import ReactDOM from 'react-dom';

const World = React.memo(function Map() {
  const [ data, setData ] = React.useState();
  React.useEffect(() => {
    const fetchData = async () => {
      const response = await fetch(
        'https://gw.alipayobjects.com/os/basement_prod/68dc6756-627b-4e9e-a5ba-e834f6ba48f8.json'
      ); 
      const data = await response.json(); // 获取geoJson，也可以使用本地的geoJson
      setData(data);
    };
    fetchData();
  }, []);
  return (
    <MapboxScene // 地图场景
      map={{
        center: [ 0.19382669582967, 50.258134 ],
        pitch: 0,
        style: 'blank', // 无底图
        zoom: 6
      }}
      style={{
        position: 'absolute',
        top: 0,
        left: 0,
        right: 0,
        bottom: 0
      }}
    >
      {data && [
        <PolygonLayer // 面图层，区域填充颜色
          key={'2'}
          options={{
            autoFit: true
          }}
          source={{
            data
          }}
          color={{
            field: 'name',
            values: [
              '#2E8AE6',
              '#69D1AB',
              '#DAF291',
              '#FFD591',
              '#FF7A45',
              '#CF1D49'
            ]
          }}
          shape={{
            values: 'fill'
          }}
          style={{
            opacity: 1
          }}
          select={{
            option: { color: '#FFD591' }
          }}
        />,
        <LineLayer // 线图层，划分区域界限
          key={'21'}
          source={{
            data
          }}
          color={{
            values: '#fff'
          }}
          shape={{
            values: 'line'
          }}
          style={{
            opacity: 1
          }}
        />
      ]}
    </MapboxScene>
  );
});

ReactDOM.render(<World/>, document.getElementById('map')); // 将地图渲染到DOM中
```