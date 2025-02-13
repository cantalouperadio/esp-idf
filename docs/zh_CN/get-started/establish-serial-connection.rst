与 {IDF_TARGET_NAME} 创建串口连接
==============================================

:link_to_translation:`en:[English]`

本章节主要介绍如何创建 {IDF_TARGET_NAME} 和 PC 之间的串口连接。


连接 {IDF_TARGET_NAME} 和 PC
------------------------------

用 USB 线将 {IDF_TARGET_NAME} 开发板连接到 PC。如果设备驱动程序没有自动安装，请先确认 {IDF_TARGET_NAME} 开发板上的 USB 转串口芯片（或外部转串口适配器）型号，然后在网上搜索驱动程序，并进行手动安装。

以下是乐鑫 {IDF_TARGET_NAME} 开发板驱动程序的链接：

* CP210x: `CP210x USB 至 UART 桥 VCP 驱动程序 <https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers>`_
* FTDI: `FTDI 虚拟 COM 端口驱动程序 <https://ftdichip.com/drivers/vcp-drivers/>`_

以上驱动仅供参考，请参考开发板用户指南，查看开发板具体使用的 USB 转串口芯片。一般情况下，当 {IDF_TARGET_NAME} 开发板与 PC 连接时，对应驱动程序应该已经被打包在操作系统中，并已经自动安装。

在 Windows 上查看端口
---------------------

检查 Windows 设备管理器中的 COM 端口列表。断开 {IDF_TARGET_NAME} 与 PC 的连接，然后重新连接，查看哪个端口从列表中消失后又再次出现。

以下为 ESP32 DevKitC 和 ESP32 WROVER KIT 串口：

.. figure:: ../../_static/esp32-devkitc-in-device-manager.png
    :align: center
    :alt: 设备管理器中 ESP32-DevKitC 的 USB 至 UART 桥
    :figclass: align-center

    设备管理器中 ESP32-DevKitC 的 USB 至 UART 桥

.. figure:: ../../_static/esp32-wrover-kit-in-device-manager.png
    :align: center
    :alt: Windows 设备管理器中 ESP-WROVER-KIT 的两个 USB 串行端口
    :figclass: align-center

    Windows 设备管理器中 ESP-WROVER-KIT 的两个 USB 串行端口


在 Linux 和 macOS 上查看端口
-----------------------------

查看 {IDF_TARGET_NAME} 开发板（或外部转串口适配器）的串口设备名称，请将以下命令运行两次。首先，断开开发板或适配器，首次运行以下命令；然后，连接开发板或适配器，再次运行以下命令。其中，第二次运行命令后出现的端口即是 {IDF_TARGET_NAME} 对应的串口：

Linux::

    ls /dev/tty*

macOS::

    ls /dev/cu.*

.. 注解::

    对于 macOS 用户：若没有看到串口，请检查是否安装 USB/串口驱动程序。具体应使用的驱动程序，见章节 `连接 {IDF_TARGET_NAME} 和 PC`_。对于 macOS High Sierra (10.13) 的用户，你可能还需要手动允许驱动程序的加载，具体可打开 ``系统偏好设置`` -> ``安全和隐私`` -> ``通用``，检查是否有信息显示：“来自开发人员的系统软件...”，其中开发人员的名称为 Silicon Labs 或 FTDI。


.. _linux-dialout-group:

在 Linux 中添加用户到 ``dialout``
-----------------------------------

当前登录用户应当可以通过 USB 对串口进行读写操作。在多数 Linux 版本中，您都可以通过以下命令，将用户添加到 ``dialout`` 组，从而获许读写权限::

    sudo usermod -a -G dialout $USER

在 Arch Linux 中，需要通过以下命令将用户添加到 ``uucp`` 组中::

    sudo usermod -a -G uucp $USER

请重新登录，确保串口读写权限生效。


确认串口连接
------------------------

现在，请使用串口终端程序，查看重置 {IDF_TARGET_NAME} 后终端上是否有输出，从而验证串口连接是否可用。

Windows 和 Linux 操作系统
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在本示例中，我们将使用 `PuTTY SSH Client <https://www.putty.org/>`_， `PuTTY SSH Client <https://www.putty.org/>`_ 既可用于 Windows 也可用于 Linux。你也可以使用其他串口程序并设置如下的通信参数。

运行终端，配置在上述步骤中确认的串口：波特率 = 115200，数据位 = 8，停止位 = 1，奇偶校验 = N。以下截屏分别展示了如何在 Windows 和 Linux 中配置串口和上述通信参数（如 115200-8-1-N）。注意，这里一定要选择在上述步骤中确认的串口进行配置。

.. figure:: ../../_static/putty-settings-windows.png
    :align: center
    :alt: 在 Windows 操作系统中使用 PuTTY 设置串口通信参数
    :figclass: align-center

    在 Windows 操作系统中使用 PuTTY 设置串口通信参数

.. figure:: ../../_static/putty-settings-linux.png
    :align: center
    :alt: 在 Linux 操作系统中使用 PuTTY 设置串口通信参数
    :figclass: align-center

    在 Linux 操作系统中使用 PuTTY 设置串口通信参数


然后，请检查 {IDF_TARGET_NAME} 是否有打印日志。如有，请在终端打开串口进行查看。这里的日志内容取决于加载到 {IDF_TARGET_NAME} 的应用程序，请参考 `输出示例`_。

.. 注解::

   请在验证完串口通信正常后，关闭串口终端。如果您让终端一直保持打开的状态，之后上传固件时将无法访问串口。

macOS 操作系统
^^^^^^^^^^^^^^^^^

macOS 提供了 **屏幕** 命令，因此您不用安装串口终端程序。

- 参考 `在 Linux 和 macOS 上查看端口`_，运行以下命令::

    ls /dev/cu.*

- 您会看到类似如下输出::

    /dev/cu.Bluetooth-Incoming-Port /dev/cu.SLAB_USBtoUART      /dev/cu.SLAB_USBtoUART7

- 根据您连接到电脑上的开发板类型和数量，输出结果会有所不同。请选择开发板的设备名称，并运行以下命令::

    screen /dev/cu.device_name 115200

将 ``device_name`` 替换为运行 ``ls /dev/cu.*`` 后出现的设备串口号。

- 您需要的正是 **屏幕** 显示的日志。日志内容取决于加载到 {IDF_TARGET_NAME} 的应用程序，请参考 `输出示例`_。请使用 Ctrl-A + \\ 键退出 **屏幕** 会话。

.. 注解::

   请在验证完串口通信正常后，关闭 **屏幕** 会话。如果直接关闭终端窗口而没有关闭 **屏幕**，之后上传固件时将无法访问串口。


输出示例
^^^^^^^^^^^

以下是 {IDF_TARGET_NAME} 的一个日志示例。如果没看到任何输出，请尝试重置开发板。

.. highlight:: none

::

    ets Jun  8 2016 00:22:57

    rst:0x5 (DEEPSLEEP_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
    ets Jun  8 2016 00:22:57

    rst:0x7 (TG0WDT_SYS_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
    configsip: 0, SPIWP:0x00
    clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
    mode:DIO, clock div:2
    load:0x3fff0008,len:8
    load:0x3fff0010,len:3464
    load:0x40078000,len:7828
    load:0x40080000,len:252
    entry 0x40080034
    I (44) boot: ESP-IDF v2.0-rc1-401-gf9fba35 2nd stage bootloader
    I (45) boot: compile time 18:48:10

    ...

如果打印出的日志是可读的（而不是乱码），则表示串口连接正常。此时，您可以继续进行安装，并最终将应用程序上载到 {IDF_TARGET_NAME}。

.. 注解::

   在某些串口接线方式下，在 {IDF_TARGET_NAME} 启动并开始打印串口日志前，需要在终端程序中禁用串口 RTS ＆ DTR 管脚。该问题仅存在于将 RTS ＆ DTR 管脚直接连接到 EN ＆ GPIO0 管脚上的情况，绝大多数开发板（包括乐鑫所有的开发板）都没有这个问题。更多详细信息，请参考 `esptool 文档`_。

如您在安装 {IDF_TARGET_NAME} 硬件开发的软件环境时，从 :ref:`get-started-connect` 跳转到了这里，请从 :ref:`get-started-configure` 继续阅读。

.. _esptool 文档: https://docs.espressif.com/projects/esptool/en/latest/advanced-topics/boot-mode-selection.html#automatic-bootloader
