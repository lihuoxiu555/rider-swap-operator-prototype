# 权益准入 API（换电前）

> 换电流程第一步：校验骑手当日是否可使用**渠道人天**或**有效个人套餐**。  
> 原型说明弹窗：`MODULE_NOTES.entitlement_api` · 骑手端演示：`channel_fail`（U2103）

---

## 1. 接口概览

| 项目 | 说明 |
|------|------|
| 路径 | `POST /api/v1/entitlement/check` |
| 调用方 | 骑手 App / 小程序（扫码换电前） |
| 幂等 | 同一 user + 自然日可缓存结果；换电 start 须再次校验 |

---

## 2. 请求

| 字段 | 类型 | 必填 | 说明 |
|------|------|:----:|------|
| `user_id` | string | Y | 骑手 ID |
| `cabinet_sn` | string | Y | 柜机 SN |
| `site_id` | string | N | 站点 ID（推荐传，用于规则命中） |
| `request_time` | datetime | N | 客户端时间；服务端以服务器时间为准 |

```json
{
  "user_id": "U2103",
  "cabinet_sn": "CAB-22018",
  "site_id": "ST-SH-01"
}
```

---

## 3. 响应

| 字段 | 类型 | 说明 |
|------|------|------|
| `allowed` | boolean | 是否允许使用渠道/套餐权益换电 |
| `entitlement_type` | enum | `day_pool` / `personal_pkg` / `none` |
| `pool_id` | string | 渠道成员所属额度池（个人用户为空） |
| `seller_operator_id` | string | 额度售卖方 U（= 换电三元组 `userOwner`） |
| `today_status` | enum | `reserved` / `confirmed` / `failed` / `pending_first_swap` / `not_allocated` |
| `fail_reason` | string | 失败原因（余额不足、未分配、规则未命中等） |
| `fallback` | object | 自费兜底推荐（见 §4） |

**成功示例（渠道已预占）**

```json
{
  "allowed": true,
  "entitlement_type": "day_pool",
  "pool_id": "QP-2601",
  "seller_operator_id": "OP-SX",
  "today_status": "reserved",
  "fail_reason": null,
  "fallback": { "enabled": false }
}
```

**失败示例（预占失败 → 引导自费）**

```json
{
  "allowed": false,
  "entitlement_type": "day_pool",
  "pool_id": "QP-2601",
  "seller_operator_id": "OP-SX",
  "today_status": "failed",
  "fail_reason": "渠道额度池余额不足，今日预占失败",
  "fallback": {
    "enabled": true,
    "recommended_skus": ["1day", "single"],
    "prices": { "1day": 29, "single": 9.9 },
    "validity_hours": 24,
    "note": "不参与合伙人分润；不扣渠道池"
  }
}
```

---

## 4. fallback 对象

| 字段 | 说明 |
|------|------|
| `enabled` | 是否允许转在线自费 |
| `recommended_skus` | 推荐 SKU：`1day`（1天畅换）、`single`（单次换电） |
| `prices` | 零售价（元），来自运营商「定价管理」渠道兜底 SKU |
| `validity_hours` | 购后有效时长（默认 24h） |
| `note` | 展示文案 |

自费换电成功后：
- **不扣**渠道额度池人天；
- 产生 C 端支付订单（如 `PAY-POOL-*`）；
- 平台 1% 在支付成功时收取；
- **不参与**合伙人分润。

---

## 5. 错误码

| 码 | 含义 | 骑手端处理 |
|----|------|------------|
| `E001` | 未登记渠道且无有效个人套餐 | 引导服务购买 |
| `E002` | 预占失败（池余额不足） | 展示原因 + fallback 自费 |
| `E003` | 无匹配额度使用规则 | 联系渠道商 |
| `E004` | 个人与渠道身份冲突 | 提示退订/冻结个人套餐 |
| `E005` | （预留）首换开通 | 首版池级固定 `on_allocate`，不返回此码 |

---

## 6. 与换电 start 的关系

```
扫码 → entitlement/check → allowed?
  ├─ true  → POST /api/v1/swap/start（带 pool_id / sub_id）
  └─ false → fallback.enabled?
        ├─ true  → 引导支付兜底 SKU → 支付成功 → swap/start（entitlement=retail_fallback）
        └─ false → 拦截，展示 fail_reason
```

跨网/跨站准入在 `swap/start` 前另调运营商策略（与本文档独立）。

---

## 7. 修订记录

| 版本 | 日期 | 说明 |
|------|------|------|
| v1.0 | 2026-06-11 | 首版；对齐 G9 / 骑手端 U2103 自费兜底演示 |
