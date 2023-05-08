

# 基础散点图

## 简述

基础散点图

![./image-20230307154613543](images/image-20230307154613543.png)

## 配置项

### 轴线

![./image-20230307155847382](images/image-20230307155847382.png)

### 图例

![image-20230307154826274](images/image-20230307154826274.png)

### 值标签

![./image-20230307154857699](images/image-20230307154857699.png)

### 散点样式

![./image-20230307155408677](images/image-20230307155408677.png)

### 悬浮提示

![image-20230307155439556](./images/image-20230307155439556.png)

## 数据模板

```json
{
    "dataModelDefinition": {
        "name": "base-scatter",
        "title": "base-scatter",
        "icon": "",
        "description": "base-scatter",
        "author": "",
        "header": {
            "dimensions": [
                {
                    "dataType": "String",
                    "fieldLabel": "维度名称",
                    "fieldName": "dimension_name",
                    "fieldUnit": "",
                    "list": "true",
                    "rowProperties": ["format"]
                },
                {
                    "dataType": "String",
                    "fieldLabel": "对比维度",
                    "fieldName": "compareType",
                    "fieldUnit": "",
                    "list": "true",
                    "rowProperties": ["format"]
                }
            ],
            "indicators": [
                {
                    "dataType": "String",
                    "fieldLabel": "指标标签",
                    "fieldName": "indicator_label",
                    "fieldUnit": "",
                    "list": "true",
                    "rowProperties": ["format"]
                },
                {
                    "dataType": "String",
                    "fieldLabel": "指标数值",
                    "fieldName": "indicator_value",
                    "fieldUnit": "",
                    "list": "true",
                    "rowProperties": ["format"]
                },
                {
                    "dataType": "String",
                    "fieldLabel": "指标单位",
                    "fieldName": "indicator_unit",
                    "fieldUnit": "",
                    "list": "true",
                    "rowProperties": ["format"]
                }
            ]
        },
        "rowConfig": {
            "dimensionCount": "unknown",
            "isUseDimensionParams": false
        }
    }
}
```

## 特殊说明

<!-- 暂无 -->
