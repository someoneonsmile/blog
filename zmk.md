# ZMK

| date       | tag            |
| ---------- | -------------- |
| 2022-07-27 | zmk / keyboard |

- `&kp`: key press

- `&mo`: momentary layer, 临时层

- `&lt`: layer tap, 长按时切换层

- `&to`: to layer, 切到层

- `&tog`: toggle layer, 切换层的状态

- `&trans`: transparent, 透明键, 使用下一个活动层的键

- `&none`: none

- hold tap

  quick-tap-ms, 配置时间, 按下, 再次长按时表现的像一直在点按

- `&mt`: mode tap, 长按表现配置的第一个键(如 ctrol), 点按表现配置的第二个键

- mod-morph

  `&gresc`: `or esc on same press gui key, 单按时表现为`, 配合 gui 一个按下时表现为 esc

- `&kt`: key toggle, 切换键是否被按下的状态

- `&sk`: sticky key, 粘滞键

- `&sl`: sticky layer, 粘滞层

- tap-dance: muti tap, 配置单击，双击, 三击的不同表现

- `&caps_word`, 切换大写锁定

- `&key_repeat`: send whaatever keycode was last sent, 重新发送上次发送的键码
