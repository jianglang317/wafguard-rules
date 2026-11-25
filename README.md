# wafguard-rules

WAFGuard 宝塔安全插件（v1.0.16+）的**体检规则包**远程源。

## 用途

插件面板「安全体检 → 规则更新」按钮会从本仓库的 `checkpack.json` 拉取最新检测规则：

- **新增**：包里出现新 key（如新披露的 CVE）→ 插件入库，下次体检即生效
- **更新**：已有 key 的定义（等级/版本区间/修复建议）→ 插件同步覆盖
- **不删除**：远端去掉的项本地保留；手动停用/忽略的项保持停用
- **校验失败整包拒绝**：本地规则不降级

Raw 地址（填到插件「规则源」设置里）：

```
https://raw.githubusercontent.com/jianglang317/wafguard-rules/main/checkpack.json
```

## 规则维护流程（出新漏洞时）

1. 编辑 `checkpack.json`：加一条检测项（probe 用 `component_version` + 版本区间），标注 CVE 编号
2. `version` 字段 +1（8 位日期号，如 `20261125` → `20261202`）
3. commit + push 到 `main` 分支
4. 宝塔面板 → WAFGuard → 安全体检 → 点「规则更新」

## 字段说明

| 字段 | 说明 |
|---|---|
| `key` | 唯一标识，`[a-z0-9_]+`，新增时不可与已有重复 |
| `level` | `高危` / `中危` / `低危` |
| `probe` | 探测类型白名单（16 种：`component_version`/`http_status`/`php_ini_flag` 等） |
| `params` | 探测参数；漏洞型用 `items: [{name, cmd, regex, min_ok, cve}]` |
| `fix_type` | `manual`（人工处置）或具体自动修复类型 |
| `grp` | `web`（站点配置）/ `sys`（系统与 PHP 基线） |
| `version` | 8 位日期号，只有比本地新才会入库 |
