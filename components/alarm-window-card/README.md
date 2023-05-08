

# 订阅告警流水窗(alarm-window-card) 组件

## 简述

-   用于支持平台告警流水详情呈现
-   订阅告警流水窗需要告警流水窗配置文件 environment.json，节点 alarmWindowConfig 配置内容与现场部署的告警监控项目的配置节点内容相同，配置方式参照告警监控项目的 environment.json、alarm-window.json
-   支持皮肤配置、告警流水窗显示标题配置
-   支持告警流水窗配置路径，配置文件路径示例：配置中心地址/configmanage/alarm-window.json?isApply=true&projectName=oss-visual-designer
-   告警流水窗缩放比例，如果启用背景画布缩放设置功能，会导致告警流水窗鼠标点击事件发生偏差显示异常，需要开启此功能将告警流水窗的缩放比例抵消

## 组件逻辑

该组件用于呈现告警流水详情、告警右键和相应的功能按钮操作等

## 配置项

## 基础配置

-   告警流水窗显示标题
-   告警流水窗显示皮肤
-   告警流水窗配置路径
-   告警流水窗缩放比例

### 数据

默认 json 数据格式，也支持接口获取数据.

### 交互

暂无
