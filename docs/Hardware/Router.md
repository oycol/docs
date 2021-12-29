## 硬件构成

CPU

RAM

- SDRAM
- DDR
- DDR2
- DDR3

ROM(Flash)

- SPI Flash
- NOR Flash
- NAND Flash

WIFI芯片

- USB 总线
- PCI-E 总线

## 软件构成

Bootloader

- CFE
- Uboot

固件

- 开源： Openwrt、tomato、DD-WRT
- 三方： VxWorks、类Unix、开源修改

### Q

- 路由器包转发能力测试

    - 64byte包吞吐量反应了CPU的性能
    - 512byte包反应了CPU和内存整体综合性能
    - 1518byte包吞吐量反应了内存性能

    吞吐量的标准单位是pps（packet per second），在MikroTik给出的Mbps和Kpps数据是可以相互转换的，例如1518Byte的31472Mbps约等于2591.6kpps * 1000 * 1518 * 8

- bootloader 
