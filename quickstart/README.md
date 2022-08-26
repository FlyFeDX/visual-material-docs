# 可视化组件物料仓

> oss-visual-material

# 环境初始化

```yaml
# 1. 下载代码仓
git clone <git repo>

# 2. 安装依赖(npm yarn pnpm)都可以，不可混用
pnpm install

# 3. 创建分支
git checkout -b <feature-任务号> develop

# 4. 导入DesignerApp
# 将designer的包放到designer的目录下，如下图所示
```

## designer app 包目录

![image-20220421174241109](images/designer-app-dir.png)

# 组件开发

## 一键创建组件命令

`yarn create-material --dir [materialpath]`

## 组件目录配置

> 组件都应该必须放到`src/packages/`下面

每个组件目录至少包含三个文件，示例如下

```md
-   packages
    -   border1
        -   index.tsx 组件代码入口
        -   index.less 组件样式代码
        -   schema.ts 组件 DSL
        -   oss-material.json 物料描述文件
```

### index.tsx

> 可以是 `index.js` , `index.jsx` 等，但是还是建议使用.tsx

文件内容描述（示例）:

```tsx | pure
interface IProps {
    /**
     * 组件唯一id
     */
    uniqueId: string;

    /**
     * designer注入的参数
     */
    readonly designer: {
        env: any; // 环境变量 useEnvironmentModel.data.environment
        constants: any; // @Common/constants
        mode: string; // preview | development
        container: HTMLElement; // designer的容器，可以把组件的modal框放入其中
        prefix: any; // 前缀的字符串集合？
        // designer的权限字段
        permissions: {
            zoneName: any; //
            zoneLevel: any; //
            zoneId: any; //
            parentZoneId: any; //
            userId: any; //
        };
    };

    /**
     * 交互对象
     */
    interaction: {
        defined: any; // 已经定义的交互字段
        state: any; // 派发的数据
        dispatch: (arg: { type?: string; data: any }) => void; // 向外部派发数据
    };

    /**
     * 接收到的交互数据
     */
    interactionProps: any;

    /**
     * 右侧面板传入的配置数据
     */
    config: Record<string, any>;

    /**
     * 右侧数据面板配置后得到的请求返回的数据
     */
    dataSource: any;
}

const Comp: FC<IProps> = (props) => {
    return <section>{/* 组件内容 */}</section>;
};
```

### schema.ts

schema 文件由三部分组成，分别为`schema`，`defaultValue`，`materialInfo`，缺一不可。

注意：

-   schema 使用的 formily 组件编写
-   编写的自由度比较高，所以请参考之前的代码编写
-   https://v2.formilyjs.org/

```ts
export const schema = {
    materials: 'border1',
    fields: [
        {
            name: '配置',
            key: 'config',
            schema: {
                type: 'object',
                properties: {
                    config: {
                        type: 'object',
                        properties: {
                            baseInfo: {
                                type: 'void',
                                'x-component': 'ComponentBaseInfo',
                                'x-component-props': {
                                    componentName: '边框1',
                                    componentersion: 'V1.0',
                                    componentType: 'border1',
                                },
                                properties: {
                                    title: {
                                        type: 'text',
                                        'x-component': 'EditableInput',
                                    },
                                },
                            },
                            divider1: {
                                type: 'void',
                                'x-component': 'Divider',
                                'x-component-props': {
                                    style: {
                                        margin: '10px 0',
                                        borderTop: '1px solid rgb(85, 85, 85)',
                                    },
                                },
                            },
                            layout: {
                                type: 'void',
                                'x-component': 'FormLayout',
                                'x-component-props': {
                                    labelCol: 7,
                                    wrapperCol: 15,
                                    labelAlign: 'left',
                                },
                                properties: {
                                    position: {
                                        type: 'void',
                                        title: '位置',
                                        'x-decorator': 'FormItem',
                                        'x-component': 'Space',
                                        'x-component-props': {
                                            size: 20,
                                        },
                                        properties: {
                                            left: {
                                                type: 'string',
                                                'x-component': 'NumberPicker',
                                            },
                                            top: {
                                                type: 'string',
                                                'x-component': 'NumberPicker',
                                            },
                                        },
                                    },
                                    size: {
                                        type: 'void',
                                        title: '尺寸',
                                        'x-decorator': 'FormItem',
                                        'x-component': 'Space',
                                        'x-component-props': {
                                            size: 0,
                                        },
                                        properties: {
                                            width: {
                                                type: 'number',
                                                'x-component': 'NumberPicker',
                                            },
                                            lockedScale: {
                                                type: 'number',
                                                'x-component': 'Lock',
                                                'x-component-props': {
                                                    style: {
                                                        width: 22,
                                                    },
                                                },
                                            },
                                            height: {
                                                type: 'number',
                                                'x-component': 'NumberPicker',
                                            },
                                        },
                                    },
                                    transform: {
                                        type: 'object',
                                        title: '角度',
                                        'x-decorator': 'FormItem',
                                        'x-component': 'FormGrid',
                                        'x-component-props': {
                                            minColumns: 4,
                                            columnGap: 20,
                                        },
                                        properties: {
                                            rotate: {
                                                type: 'string',
                                                'x-component': 'NumberPicker',
                                                'x-decorator': 'FormItem',
                                                'x-decorator-props': {
                                                    gridSpan: 2,
                                                    feedbackLayout: 'none',
                                                },
                                            },
                                            scale: {
                                                type: 'string',
                                                'x-component': 'TileSelect',
                                                'x-decorator': 'FormItem',
                                                'x-decorator-props': {
                                                    gridSpan: 2,
                                                    feedbackLayout: 'none',
                                                },
                                                'x-component-props': {
                                                    columns: 2,
                                                    rows: 1,
                                                    enums: [
                                                        [
                                                            {
                                                                label: '',
                                                                value: '-1, 1',
                                                                icon: 'visual-manager-symmetric',
                                                            },
                                                            {
                                                                label: '',
                                                                value: '1, -1',
                                                                icon: 'visual-manager-symmetric-copy',
                                                            },
                                                        ],
                                                    ],
                                                    cancelable: true,
                                                },
                                            },
                                        },
                                    },
                                    opacity: {
                                        type: 'string',
                                        title: '不透明度',
                                        'x-decorator': 'FormItem',
                                        'x-component': 'Slider',
                                    },
                                    group: {
                                        type: 'void',
                                        'x-component': 'FormGrid',
                                        'x-component-props': {
                                            minColumns: [2],
                                            maxColumns: [2],
                                        },
                                        properties: {
                                            isLock: {
                                                type: 'boolean',
                                                title: '锁定图层',
                                                'x-decorator': 'FormItem',
                                                'x-decorator-props': {
                                                    labelCol: 14,
                                                    wrapperCol: 10,
                                                },
                                                'x-component': 'Switch',
                                            },
                                            isHidden: {
                                                type: 'boolean',
                                                title: '隐藏图层',
                                                'x-decorator': 'FormItem',
                                                'x-decorator-props': {
                                                    labelCol: 14,
                                                    wrapperCol: 10,
                                                },
                                                'x-component': 'Switch',
                                            },
                                        },
                                    },
                                },
                            },
                        },
                    },
                },
            },
        },
    ],
};

export const defaultValue = {
    config: {
        title: '边框1',
        width: 400,
        height: 250,
        left: 15,
        top: 15,
        background: '',
        isLock: false,
        isHidden: false,
    },
};

export const materialInfo = {
    name: '边框1',
    icon: 'border1',
    type: 'border1',
};
```

### oss-material.json

```json
{
    "version": "0.0.1",
    "main": "./index.js",
    "schema": "./schema.ts",
    "dataModel": ""
}
```

### index.less

> 样式文件

### dataModel.json

> 组件数据描述

对于需要对接接口数据的组件,组件的数据描述必须提供.

-   作用 1,描述组件所需的数据结构
-   作用 2,使用数据集作为数据源时,作为数据映射关键文件.

    ![image-20220422090639147](images/datamodel.png)

## 组件说明文档

> 组件说明文档,作为组件开发的重要交付物存在.

```js
   目录结构:
    └── packages
        └── compXXXX
           └── doc
              ├──images
              └── README.md

```

1.  文档本地预览:

        - 在 vscode 中预览,可以按需安装 md 预览插件.
        - 在本地预览完整文档:

            ```yaml
            yarn build:doc

            修改docs 文件夹下的  components.md 文件,增加新增加的组件文档路径.

            npm i docsify-cli -g

            docsify serve docs
            ```

            默认访问地址 http://localhost:3000

        -

2.  整体文档推送  
     项目下的 docs 文件夹下的文件， 需要提交到

        https://github.com/FlyFeDX/visual-material-docs

        perssonal access token: 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

3.  文档线上访问地址

    https://flyfedx.github.io/visual-material-docs

## 组件注册

注册组件到本地 designer app，完成开发和联调。如下图所示：

![image-20220422090639147](images/register-component.png)

具体文档如下：

```JSON
{
    "thumbnail": "stack-column",
    "groupId": 2,
    "type": 1,

    "id": 9, // 必填
    "name": "border1", // 必填
    "nameCn": "border1", // 必填
    "jsPath": "http://localhost:9900/static/material-components/border1/index.js",  // 必填
    "cssPath": "http://localhost:9900/static/material-components/border1/index.css",  // 必填
    "schemaPath": "http://localhost:9900/static/material-components/border1/schema.js"  // 必填
}
```

# 组件测试

## 使用 designer 源码测试

文件路径：.env.development

![image-20220427172318590](images/dev-designer-code.png)

## 使用本地测试

编译 designer

```yaml
# 1. 启动组件开发服务
yarn start

# 2. 启动DesignerApp
yarn start:designer
```

# 组件注册

示意图如下：

<img src="images/image-20220509104208990.png" alt="image-20220509104208990" style="zoom: 80%;" />

## 重点说明

-   组件名称 +　缩略图

    <img src="images/image-20220509104528832.png" alt="image-20220509104528832" style="zoom: 67%;" />

-   组件文件路径

    可视化组件服务地址：http://10.10.2.8:4011/static/material-components

    组件目录：

    <img src="images/image-20220509104823902.png" alt="image-20220509104823902" style="zoom: 50%;" />
