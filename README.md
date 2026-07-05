# 校园3D地图导航系统

基于Vue 3 + Three.js + Pinia的校园3D地图导航系统

## 项目结构

```
campusMap/
├── public/
│   └── models/
│       └── sendRiver_exp.glb          # 3D模型文件
├── src/
│   ├── assets/                        # 静态资源
│   ├── components/                    # Vue组件
│   │   ├── scene/                     # 3D场景相关组件
│   │   │   ├── CampusScene.vue        # 主3D场景容器
│   │   │   └── BuildingMarker.vue     # 建筑标记点
│   │   └── ui/                        # UI组件
│   │       ├── SearchBar.vue          # 搜索框
│   │       ├── NavPanel.vue           # 导航面板
│   │       └── InfoModal.vue          # 信息弹窗
│   ├── composables/                   # 组合式函数
│   │   ├── useThree.js                # Three.js核心逻辑
│   │   ├── useCamera.js               # 相机控制
│   │   └── useRaycaster.js            # 射线检测
│   ├── data/                          # 数据配置
│   │   └── buildings.js               # 建筑数据配置
│   ├── router/                        # 路由配置
│   │   └── index.js
│   ├── stores/                        # Pinia状态管理
│   │   ├── buildingStore.js           # 建筑数据Store
│   │   ├── sceneStore.js              # 场景状态Store
│   │   ├── navStore.js                # 导航状态Store
│   │   └── uiStore.js                 # UI状态Store
│   ├── utils/                         # 工具函数
│   │   ├── buildingMatcher.js         # 建筑匹配逻辑
│   │   ├── debugBuilding.js           # 调试工具
│   │   └── coordinate.js              # 坐标转换工具
│   ├── views/                         # 页面视图
│   │   └── MapView.vue                # 地图页面
│   ├── App.vue                        # 根组件
│   └── main.js                        # 入口文件
├── index.html                         # HTML模板
├── package.json                       # 项目配置
├── vite.config.js                     # Vite配置
└── framework.md                       # 项目规划文档
```

## 技术栈

- **Vue 3**: 前端框架
- **Three.js**: 3D渲染引擎
- **Pinia**: 状态管理
- **Vue Router**: 路由管理
- **Vite**: 构建工具

## 建筑数据

系统包含12个建筑：

| Mesh名称 | 建筑ID | 建筑名称 | 类型 |
|----------|--------|----------|------|
| activitybuilding | ACT001 | 活动中心 | 活动设施 |
| cafeteria | CAN001 | 学生食堂 | 生活设施 |
| lecturehall | LEC001 | 大讲堂 | 教学设施 |
| library | LIB001 | 图书馆 | 学习设施 |
| multibuilding | MUL001 | 综合楼 | 综合设施 |
| northdorm | DORM_N001 | 北宿舍区 | 生活设施 |
| officebuilding | OFF001 | 办公楼 | 行政设施 |
| playground | PLAY001 | 操场 | 运动设施 |
| schoolbuilding1 | SCH001 | 教学楼一 | 教学设施 |
| southdorm | DORM_S001 | 南宿舍区 | 生活设施 |
| teachercanteen | CAN_T001 | 教师食堂 | 生活设施 |
| teachingbuilding | TEA001 | 教学楼 | 教学设施 |

## 安装与运行

### 安装依赖

```bash
npm install
```

### 开发模式

```bash
npm run dev
```

### 构建生产版本

```bash
npm run build
```

### 预览生产版本

```bash
npm run preview
```

## 核心功能

### 1. 3D场景渲染
- 使用Three.js加载GLB模型
- 支持相机控制（旋转、缩放、平移）
- 光照和阴影效果

### 2. 建筑标注
- 基于mesh名称自动匹配建筑数据
- 显示建筑名称标签
- 支持点击和悬停交互

### 3. 搜索功能
- 按建筑名称搜索
- 支持模糊匹配
- 显示搜索结果列表

### 4. 建筑信息
- 显示建筑详细信息
- 包含类型、楼层、开放时间、设施等
- 支持导航功能

### 5. 导航功能
- 选择起点和终点
- 显示路线信息
- 距离和时间估算

## 数据映射原理

1. **Blender建模**: 为每个建筑mesh命名（如teachingbuilding、library等）
2. **导出GLB**: 将模型导出为GLB格式
3. **Three.js加载**: 使用GLTFLoader加载模型
4. **名称匹配**: 通过mesh名称查找对应的建筑数据
5. **建立映射**: 创建mesh与建筑数据的映射关系
6. **交互显示**: 在3D场景中显示建筑标注

## Store状态管理

### buildingStore
- 管理所有建筑数据
- 提供搜索、筛选功能
- 处理建筑选择和高亮

### sceneStore
- 管理场景加载状态
- 处理加载进度
- 错误管理

### navStore
- 管理导航状态
- 存储起点和终点
- 路线信息

### uiStore
- 管理UI状态
- 控制面板显示
- 主题和语言设置

## 开发说明

### 添加新建筑

1. 在Blender中创建建筑mesh并命名
2. 在`src/data/buildings.js`中添加建筑数据
3. 重新导出GLB模型
4. 系统会自动匹配新建筑

### 调试工具

使用`src/utils/debugBuilding.js`中的调试函数：

```javascript
import { debugListAllMeshes, debugBuildingMapping } from '@/utils/debugBuilding.js';

debugListAllMeshes(model);
debugBuildingMapping(buildingMap);
```

## 注意事项

- 确保GLB模型文件位于`public/models/`目录
- mesh名称必须与配置文件中的key匹配
- 非建筑对象会被自动过滤（background、camera、Light等）
- 建议使用现代浏览器以获得最佳性能

## 许可证

MIT License
