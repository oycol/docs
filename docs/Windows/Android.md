安装twrp

进入开发者模式，开启调试模式

```shell
adb devices 查看设备连接状态
adb reboot bootloader  进入bootloader
fastboot flash recovery twrp.img  刷入twrp
## 不用同时执行
fastboot boot twrp.img # 此行命令可以进TWRP的rec（或者刷入TWRP后重启前按音量加减和电源键进手机rec模式）
fastboot reboot
```



