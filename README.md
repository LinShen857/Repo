# XRobot 模块源（LinShen857）

这个仓库只放一个 `index.yaml`：它把 `LinShen857/<Repo>` 映射到各模块的 Git 仓库地址。其他工程把本仓库的 `index.yaml` 加进自己的 `Modules/sources.yaml`，就能用 `xrobot module add LinShen857/XXX` 复用这些模块。

## 模块清单

| 模块 | 作用 | 依赖 |
| --- | --- | --- |
| `LinShen857/CanDevice` | CAN 设备抽象基类（库模块，`standalone: false`） | — |
| `LinShen857/CanRouter` | 多条 FDCAN 的接收分发与总线错误广播 | CanDevice |
| `LinShen857/Motor_GM` | 大疆 GM2006 / GM3508 / GM6020 | CanDevice, CanRouter |
| `LinShen857/Motor_DM` | 达妙 DM 电机（MIT 协议） | CanDevice, CanRouter |
| `LinShen857/PowerMeter` | CAN 功率计 | CanDevice, CanRouter |
| `LinShen857/Imu` | BMI088 + VQF 姿态解算 | — |
| `LinShen857/Dbus` | DJI DBUS 遥控接收 | — |

## 地址两种写法

本地开发（模块仓库就在本机）：

```yaml
namespace: LinShen857
modules:
  - id: LinShen857/Motor_GM
    repo: C:/stm32/xr-modules/Motor_GM
```

推到 GitHub 之后：

```yaml
namespace: LinShen857
modules:
  - id: LinShen857/Motor_GM
    repo: https://github.com/LinShen857/Motor_GM.git
```

## 在其他工程里引用

```bash
xrobot source add-source https://raw.githubusercontent.com/LinShen857/Repo/main/index.yaml --priority 1
xrobot module add LinShen857/Motor_GM@v1.0.0
xrobot setup
```

给模块打 tag 之后再引用 `@v1.0.0`，别长期挂在 `same-or-dev` 上，否则各工程会被你的分支改动牵连。

## 发布一个模块的流程

```bash
cd c:\stm32\xr-modules\Motor_GM
git remote add origin https://github.com/LinShen857/Motor_GM.git
git push -u origin main
git tag v1.0.0
git push origin v1.0.0
```

七个模块仓库分别是 `CanDevice`、`CanRouter`、`Motor_GM`、`Motor_DM`、`PowerMeter`、`Imu`、`Dbus`，每个都是独立的 Git 仓库（XRobot 约定：一个仓库 = 一个模块 = 一个同名全局类）。
