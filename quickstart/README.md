# 可视化组件物料仓

> oss-visual-material

# 写在最前

组件开发重点交付物:

-   index.js/ index.jsx /index.tsx
-   schema.ts
-   oss-material.json
-   组件说明文档 /doc
-   代码提交前,需要注册到开发服务器,方法见下文.

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

```yaml
# 创建物料
# 示例：yarn material:create --dir="xxxx-gis"
# 释义：在packages目录下面创建oss-gis目录和其他组件文件(tsx,less等)
yarn material:create --dir="路径"

# 在某个路径下创建物料文档
# 示例：yarn material:create --doc --dir="xxxx-gis"
# 释义：在oss-gis目录下面创建doc目录和readme.md文件
yarn material:create --doc --dir="路径"

# 更新物料（维护版本信息、oss-material.json相关信息，自动创建、更新changelog，）
# - 根据提示填写信息
#   - 更新版本
#   - 提交物料的更新信息
#   - 提交本次commit的提交
yarn material:update
```

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
// 详细定义可以在 @fedex-vis/designer-types
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
            /**
             * userInfo 为登录用户信息，经过JSON.prase(userInfo),可得到object对象
             * activeRoute为当前路由信息
             * 应用样例：src\packages\alarm-window-card\index.jsx
             */
            currentLoginInfo: { userInfo: string; activeRoute: string };
        };
        // 可以用来缓存一些静态上下文的
        cache: FieldCache;
        // 系统内置的字段，用于组件事件派发
        fieldName: Record<string, string>;
        utils: {
            // 系统打log的方案
            logger: any;
            // 系统内部有转化数据的逻辑，所以可能裁剪一部分数据
            // 使用这个方法可以获取到原始数据
            getDesignerOriginData: (data) => any;
            // 遍历参数的方法 会自动注入一段当前时间的
            eachRequestParams: (params: any, callback: any) => void;
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
     * 系统注入到组件中的api
     */
    readonly api: {
        /**
         * 通用自定义接口，应用样例： src\packages\export-btn\index.tsx
         */
        customDataSourceApi: (
            type: 'api' | 'dataset',
            params: {
                /**
                 * extraConfig.responseType = blob  时，执行导出接口
                 */
                extraConfig: any;
                [prop: string]: any;
            },
        ) => Promise<any>;

        /**
         * 系统中使用的请求
         * 用来替代物料仓声明的api
         */
        request: (...args: any) => Promise<any>;
    };

    /**
     * 自定义接口接收到的交互数据
     */
    interactionProps: {
        /**自定义接口参数值，格式为{paramKey:paramValue}
         * 应用样例： src\packages\export-btn\index.tsx
         */
        customDataSourceApiParams: any;
        [prop: string]: any;
    };

    /**
     * 右侧面板传入的配置数据
     */
    config: Record<string, any>;

    /**
     * 右侧数据面板配置后得到的请求返回的数据
     */
    dataSource: any;

    /**
     * 右侧配置自定义数据数据面板配置后得到的请求返回的数据
     * 应用样例： src\packages\export-btn\index.tsx
     */
    customDataSourceApiConfig: any;
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
    "dataModel": "",
    "groupId": 1,
    "type": 1
}
```

**注意:groupId/type 必须为数字,否则组件无法注册到服务器.**

### index.less

> 样式文件

### dataModel.json

> 组件数据描述

对于需要对接接口数据的组件,组件的数据描述必须提供.

-   作用 1,描述组件所需的数据结构
-   作用 2,使用数据集作为数据源时,作为数据映射关键文件.

    ![image-20220422090639147](images/datamodel.png)

## 组件说明文档

> 组件说明文档,作为组件开发的重要交付物存在,在新增组件/修改了组件配置项/新增了组件能力的 情况下,组件说明文档都需同步修改.

```js
   目录结构:
    └── packages
        └── compXXXX
           └── doc
              ├── images 文档中引用的图片
              └── README.md 文档正文

```

1.  文档本地预览:

    -   在 vscode 中预览,安装 md 预览插件,即可预览写好的 md 文档.
    -   在本地预览完整文档:

        -   修改 docs 文件夹下的 **\_sidebar.md**文件,增加新增的组件文档路径.

            ![image-20220422090639147](images/image_20220919103102.png)

        -   文档路径增加完成后,依次执行以下命令

            ```yaml
            yarn build:doc

            npm i docsify-cli -g

            docsify serve docs
            ```

        -   默认访问地址 http://localhost:3000

2.  说明文档必须包含的内容:组件具有的能力/配置方式/特殊说明的内容,需要图文结合

3.  文档线上访问地址

    https://flyfedx.github.io/visual-material-docs

4.  图表类文档设计拓展阅读地址

    https://antv.antfin.com/zh-cn/vis/chart

5.  文档分组使用 frontmatter 进行管理，所以需要在文档头部定义组件名称、分组信息

    ```md
    ---
    title: [组件名称]
    group:
        title: [分组名称]
    ---

    # [组件名称]

    ## 简述

    <!-- TODO -->

    ## 配置项

    <!-- TODO -->

    ## 特殊说明
    ```

## 组件注册到本地

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

# 组件打包

> 输出单个组件包，用来上传到我们的系统中

```bash
# 执行下面的脚本，会把所有的组件分别打包出来，然后单独压缩
yarn build:single-material

# 输出指定类型的组件，不同组件之间用空格隔开
# 指定输出oss-gis，baidu-map等两个物料，示例如下：
yarn build:single-material oss-gis baid-map
```

执行过程示例如下图所示：

![image-20221215194151718](./images/image-20221215194151718.png)

组件包格式说明：

![image-20221206144023401](./images/image-20221206144023401.png)

# 组件测试

## 使用 designer 源码测试

文件路径 1：.env.development

![image-20220427172318590](images/dev-designer-code.png)

## 使用本地测试

编译 designer

```yaml
# 1. 启动组件开发服务

## 1.1 组件访问路径为：http://localhost:4001/static/material-components/alarm-window-card/0.0.1/index.js，启动命令如下：
yarn start

##1.2 组件访问路径为：http://localhost:4001/static/material-components/alarm-window-card/index.js，启动命令如下：
yarn start:noVersion

# 2. 启动DesignerApp
yarn start:designer
```

# 组件注册到服务器

## 组件管理 1.0 版本

示意图如下：

注册地址:http://10.10.2.8:9001/

登录后,进入组件管理页面,路径 **可视化管理->组件管理**

![参数值配置](images/image_16635514769304.png)

组件订阅界面

![参数值配置](images/image-20220509104208990.png)

### 重点说明

-   组件名称 +　缩略图

    ![参数值配置](images/image-20220509104528832.png)

*   组件文件路径

        可视化组件服务地址：http://10.10.2.8:4011/static/material-components

        组件目录：

    ![参数值配置](images/image-20220509104823902.png)

## 组件管理 2.0

### 组件注册方式 1

```yaml
yarn build --URL=http://10.10.5.122:7008
```

打包过程中,调用组件注册接口,完成组件注册.其中 URL 为组件注册接口所在服务器地址. 注册完成的组件,在界面[可视化物料]下可见.

该方式适用于自动化构建.

注意:如果打包完成后,新增的可视化物料不可见,可在[可视化物料]界面,点击同步按钮,进行组件注册信息同步.

![参数值配置](images/comp-sync.png)

### 组件注册方式 2

通过[可视化物料]界面,进行物料注册.该方式适用于组件手动打包部署过程,例如组件仓由第三方维护的情况.

![参数值配置](images/comp-registe.png)
