---
layout: bidder
title: AdKernel
description: Prebid AdKernel Bidder Adaptor
biddercode: adkernel
tcfeu_supported: true
dsa_supported: false
gvl_id: 14
usp_supported: true
coppa_supported: true
gpp_sids: tcfeu, usp
schain_supported: true
dchain_supported: false
userId: all
media_types: banner, video, native
safeframes_ok: true
deals_supported: false
floors_supported: true
fpd_supported: true
pbjs: true
pbs: true
pbs_app_supported: true
prebid_member: false
multiformat_supported: will-bid-on-any
ortb_blocking_supported: true
privacy_sandbox: no
sidebarType: 1
---

### Note

The Adkernel Bidding adaptor requires setup and approval before beginning. Please reach out to <prebid@adkernel.com> for more details

### Bid Params

{: .table .table-bordered .table-striped }
| Name     | Scope    | Description           | Example                   | Type     |
|----------|----------|-----------------------|---------------------------|----------|
| `host`   | required | Ad network's RTB host | `'cpm.metaadserving.com'` | `string` |
| `zoneId` | required | RTB zone id           | `30164`                 | `integer` |
