---
title: "Listeners"
date: 2021-06-12T12:37:41+01:00
description: "There are a lot of listeners in discord.py, and while most are well documented and explained in discord.py's docs, some (for whatever reason) are not."
---

{{< tip "warning" >}}
If a listener is undocumented you almost certainly do not need it, and will very very rarely require its use. This is as much a reference for myself to simply remember these listeners as it is meant to be useful to others. Use these listeners with care and understanding, if you didn't come here looking for them you probably don't need them, and should turn back.
{{< /tip >}}

{{< tip "info" >}}
This is not a detailed guide on how to use these listeners, it is just a reference to their existance.
{{< /tip >}}

# `on_socket_response`

Triggers on every gateway receive event. This function takes one payload argument which will be directly given the gateway data from Discord, with the fields `op`, `s`, `d`, and `t`.
