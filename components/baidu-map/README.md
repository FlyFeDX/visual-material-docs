# 百度地图(baidu-map) 组件

## 简述

-   用于支持平台百度地图呈现.
-   百度地图 ak 需要配置，样式 ID 可选，有默认值。
-   其他属性按需配置。

## 上述特殊情况说明

只支持百度地图呈现，若需呈现其他地图，请使用 oss-gis 组件。标注图标根据数据源中 neType,alarmLevel 共同决定。

```js
let baseUrl = `${constants.IMAGE_PATH}/map/{0}/{1}.png`;
if (point.imgPath !== undefined) {
    imgPath = point.imgPath;
} else {
    imgPath = point.alarmLevel ? format(baseUrl, [point.neType, parseFloat(point.alarmLevel)]) : format(baseUrl, [point.neType, 0]);
}
```

```js
如果需要标注点左键点击事件,需要派发出的参数为:
{
    state:{},
}
```

## 组件逻辑

该组件用于呈现百度地图以及基础点标注，目前并无复杂业务。

## 配置项

## 基础配置

-   百度地图 ak
-   地图样式 Id
-   定位经度
-   定位纬度
-   定位层级
-   最小层级
-   最大层级
-   点标注样式
-   tooltip 弹窗样式

### 数据

默认 json 数据格式，也支持接口获取数据.

### 交互

点击点标注，传递单击点数据.

## 更新说明

分组 ID: 2, 组件 ID: 224

```

```
