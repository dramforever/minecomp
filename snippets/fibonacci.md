还是一样，"Redstone Ready" 地图，放下这些命令方块，然后在 `(0, 56, 0)` 放一个红石块（就是 6 个命令方块的那行），可以看到一个二进制 Fibbonacci。`(2, 56, x)` 这一行比较适合观察。

原理就是循环。`row(t)` 代表 `(t, 56, x)` 这一行的话，循环主体就是不停 `row(-1) <- row(1) + row(2)`，`row(0)` 作进位，然后复制 `row(2) <- row(1)`，`row(1) <- row(-1)`。

加法器也是用一个循环实现的，所以会看到旁边有谜之小船。每一次循环是一个全加法器，三数 xor 用的方法是在三个数里找到两个相同的（一定存在），则另一个数就是结果；进位的话就是找两个 1，若有则结果为 1，否则 0。

懒得自动续位数了，要增加位数的话，将 40 改成更大就好。

要清理现场嘛……

```
kill @e[name=Pointer]
fill -1 56 5 2 56 41 air
```

至于为什么是 41，啊，因为多个进位。

还有，听说 `gamerule logAdminCommands false` 有奇效。

最后才是重头戏：

```
setblock 0 56 -1 command_block 2
blockdata 0 56 -1 {Command: "setblock 0 56 0 air"}
setblock 0 56 -2 chain_command_block 2
blockdata 0 56 -2 {Command: "summon Boat -3 56 5 {CustomName: Pointer}"}
setblock 0 56 -3 chain_command_block 2
blockdata 0 56 -3 {Command: "setblock 0 56 5 wool 15"}
setblock 0 56 -4 chain_command_block 2
blockdata 0 56 -4 {Command: "setblock 1 56 5 wool 0 keep"}
setblock 0 56 -5 chain_command_block 2
blockdata 0 56 -5 {Command: "fill -1 56 5 2 56 40 wool 15 keep"}
setblock 0 56 -6 chain_command_block 2
blockdata 0 56 -6 {Command: "setblock 1 56 0 redstone_block"}
setblock 1 56 -1 command_block 2
blockdata 1 56 -1 {Command: "setblock 1 56 0 air"}
setblock 1 56 -2 chain_command_block 2
blockdata 1 56 -2 {Command: "execute @e[name=Pointer] ~ ~ ~ detect 0 56 ~ wool 15 execute @e[name=Pointer] ~ ~ ~ detect 1 56 ~ wool 15 clone 2 56 ~ 2 56 ~ -1 56 ~"}
setblock 1 56 -3 chain_command_block 2
blockdata 1 56 -3 {Command: "execute @e[name=Pointer] ~ ~ ~ detect 0 56 ~ wool 0 execute @e[name=Pointer] ~ ~ ~ detect 1 56 ~ wool 0 clone 2 56 ~ 2 56 ~ -1 56 ~"}
setblock 1 56 -4 chain_command_block 2
blockdata 1 56 -4 {Command: "execute @e[name=Pointer] ~ ~ ~ detect 0 56 ~ wool 15 execute @e[name=Pointer] ~ ~ ~ detect 2 56 ~ wool 15 clone 1 56 ~ 1 56 ~ -1 56 ~"}
setblock 1 56 -5 chain_command_block 2
blockdata 1 56 -5 {Command: "execute @e[name=Pointer] ~ ~ ~ detect 0 56 ~ wool 0 execute @e[name=Pointer] ~ ~ ~ detect 2 56 ~ wool 0 clone 1 56 ~ 1 56 ~ -1 56 ~"}
setblock 1 56 -6 chain_command_block 2
blockdata 1 56 -6 {Command: "execute @e[name=Pointer] ~ ~ ~ detect 1 56 ~ wool 15 execute @e[name=Pointer] ~ ~ ~ detect 2 56 ~ wool 15 clone 0 56 ~ 0 56 ~ -1 56 ~"}
setblock 1 56 -7 chain_command_block 2
blockdata 1 56 -7 {Command: "execute @e[name=Pointer] ~ ~ ~ detect 1 56 ~ wool 0 execute @e[name=Pointer] ~ ~ ~ detect 2 56 ~ wool 0 clone 0 56 ~ 0 56 ~ -1 56 ~"}
setblock 1 56 -8 chain_command_block 2
blockdata 1 56 -8 {Command: "execute @e[name=Pointer] ~ ~ ~ setblock 0 56 ~1 wool 15"}
setblock 1 56 -9 chain_command_block 2
blockdata 1 56 -9 {Command: "execute @e[name=Pointer] ~ ~ ~ detect 0 56 ~ wool 0 execute @e[name=Pointer] ~ ~ ~ detect 1 56 ~ wool 0 setblock 0 56 ~1 wool 0"}
setblock 1 56 -10 chain_command_block 2
blockdata 1 56 -10 {Command: "execute @e[name=Pointer] ~ ~ ~ detect 0 56 ~ wool 0 execute @e[name=Pointer] ~ ~ ~ detect 2 56 ~ wool 0 setblock 0 56 ~1 wool 0"}
setblock 1 56 -11 chain_command_block 2
blockdata 1 56 -11 {Command: "execute @e[name=Pointer] ~ ~ ~ detect 1 56 ~ wool 0 execute @e[name=Pointer] ~ ~ ~ detect 2 56 ~ wool 0 setblock 0 56 ~1 wool 0"}
setblock 1 56 -12 chain_command_block 2
blockdata 1 56 -12 {Command: "setblock 2 56 0 redstone_block"}
setblock 1 56 -13 chain_command_block 2
blockdata 1 56 -13 {Command: "tp @e[name=Pointer] ~ ~ ~1"}
setblock 2 56 -1 command_block 2
blockdata 2 56 -1 {Command: "setblock 2 56 0 air"}
setblock 2 56 -2 chain_command_block 2
blockdata 2 56 -2 {Command: "execute @e[name=Pointer] ~ ~ ~ detect 1 56 ~ wool -1 setblock 1 56 0 redstone_block"}
setblock 2 56 -3 chain_command_block 2
blockdata 2 56 -3 {Command: "execute @e[name=Pointer] ~ ~ ~ detect 1 56 ~ air -1 setblock 3 56 0 redstone_block"}
setblock 2 56 -4 chain_command_block 2
blockdata 2 56 -4 {Command: "execute @e[name=Pointer] ~ ~ ~ detect 1 56 ~ air -1 kill @e[name=Pointer]"}
setblock 3 56 -1 command_block 2
blockdata 3 56 -1 {Command: "setblock 3 56 0 air"}
setblock 3 56 -2 chain_command_block 2
blockdata 3 56 -2 {Command: "clone 1 56 5 1 56 40 2 56 5"}
setblock 3 56 -3 chain_command_block 2
blockdata 3 56 -3 {Command: "clone -1 56 5 -1 56 40 1 56 5"}
setblock 3 56 -4 chain_command_block 2
blockdata 3 56 -4 {Command: "setblock 0 56 0 redstone_block"}
```
