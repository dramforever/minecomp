使用地图超平坦 "Redstone Ready"，把自己 `/tp` 到 `0 56 0` 附近，然后执行脚本。在 `0~2 56 5` 放上黑或白色羊毛（黑 0 白 1）表示数字，然后在 `0 56 0` 放上红石块，就会在 `m 56 7` 处放上一个羊毛，其中 `m` 是由数字确定的位置。形式化地说，`m = sum(i = 0 to 2) { 2^i * ((i, 56, 5) is white wool) }`。

看起来这个过程可以在一个 tick 内完成。


```
setblock 0 56 -1 command_block 2
blockdata 0 56 -1 {Command: "setblock 0 56 0 air"}
setblock 0 56 -2 chain_command_block 2
blockdata 0 56 -2 {Command: "summon Boat 0 56 12 {CustomName: Pointer}"}
setblock 0 56 -3 chain_command_block 2
blockdata 0 56 -3 {Command: "execute @p 0 56 0 detect 0 56 5 wool 0 tp @e[name=Pointer] ~1 ~ ~"}
setblock 0 56 -4 chain_command_block 2
blockdata 0 56 -4 {Command: "execute @p 0 56 0 detect 1 56 5 wool 0 tp @e[name=Pointer] ~2 ~ ~"}
setblock 0 56 -5 chain_command_block 2
blockdata 0 56 -5 {Command: "execute @p 0 56 0 detect 2 56 5 wool 0 tp @e[name=Pointer] ~4 ~ ~"}
setblock 0 56 -6 chain_command_block 2
blockdata 0 56 -6 {Command: "execute @e[name=Pointer] ~ ~ ~ setblock ~ 56 7 wool 15"}
setblock 0 56 -7 chain_command_block 2
blockdata 0 56 -7 {Command: "kill @e[name=Pointer]"}
```
