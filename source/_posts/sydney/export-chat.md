---
title: 导出newBing聊天记录
date: 2023-05-06 17:33:15
tags: 
- freeSydney
- newBing
categories:
- freeSydney
- newBing
---
# 导出newBing聊天记录

[:heavy_check_mark:get]{.label .success}
```javascript 直接在控制台使用该命令
console.log([...document.querySelector('cib-serp').shadowRoot.querySelector('cib-conversation').shadowRoot.querySelectorAll('cib-chat-turn')].map(node=>[...node.shadowRoot.children]).flat(1).filter(t=>t?.getAttribute?.('source')).map(t=>[t.getAttribute('source'),[...t.shadowRoot.children]]).map(([from,ms])=>ms&&[from,ms.map(m=>m.shadowRoot&&[...m.shadowRoot?.children]).flat(1).filter(m=>!m?.classList.contains('hidden')&&m?.shadowRoot).map(m=>m.innerText).join('\n')]).map(([from,text])=>`[${from}] ${text}`).join('\n').replaceAll('\n已发送电子邮件.','').replaceAll('\n已收到消息.',''))
```