---
title: "使用 C 将 Raspberry Pi 预配到远程监视 - Azure | Microsoft Docs"
description: "介绍如何使用以 C 编写的应用程序将 Raspberry Pi 设备连接到 Azure IoT 套件预配置远程监视解决方案。"
services: iot-suite
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2017
ms.author: dobett
ms.openlocfilehash: 2f5915093a0d7984f0350af21aa438cdd588bbf2
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2017
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-preconfigured-solution-c"></a>将 Raspberry Pi 设备连接到远程监视预配置解决方案 (C)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

本教程介绍如何将物理设备连接到远程监视预配置解决方案。 与受约束设备上运行的大多数嵌入式应用程序一样，Raspberry Pi 设备应用程序的客户端代码是用 C 语言编写的。在本教程中，将在运行 Raspbian OS 的 Raspberry Pi 上生成应用程序。

### <a name="required-hardware"></a>所需硬件

一个台式机，用于通过远程方式连接到 Raspberry Pi 上的命令行。

[适用于 Raspberry Pi 3 的 Microsoft IoT 初学者套件](https://azure.microsoft.com/develop/iot/starter-kits/)或等效组件。 本教程使用套件中的以下项目：

- Raspberry Pi 3
- MicroSD 卡（带 NOOBS）
- USB 迷你电缆
- 以太网电缆

### <a name="required-desktop-software"></a>所需的桌面软件

需要在台式机上安装 SSH 客户端，才能远程访问 Raspberry Pi 上的命令行。

- Windows 不包括 SSH 客户端。 建议使用 [PuTTY](http://www.putty.org/)。
- 大多数 Linux 分发版和 Mac OS 包括命令行 SSH 实用工具。 有关详细信息，请参阅 [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md)（使用 Linux 或 Mac OS 的 SSH）。

### <a name="required-raspberry-pi-software"></a>所需的 Raspberry Pi 软件

以下步骤演示如何准备 Raspberry Pi 以生成可连接到预配置解决方案的 C 应用程序：

1. 使用 `ssh` 连接到 Raspberry Pi。 有关详细信息，请参阅 [Raspberry Pi 网站](https://www.raspberrypi.org/)上的 [SSH（安全外壳）](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md)。

1. 使用以下命令更新 Raspberry Pi：

    ```sh
    sudo apt-get update
    ```

1. 使用以下命令将所需的开发工具和库添加到 Raspberry Pi：

    ```sh
    sudo apt-get install g++ make cmake gcc git
    ```

1. 使用以下命令安装 IoT 中心客户端库：

    ```sh
    grep -q -F 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
    grep -q -F 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys FDA6A393E4C2257F
    sudo apt-get update
    sudo apt-get install -y azure-iot-sdk-c-dev cmake libcurl4-openssl-dev git-core
    ```

1. 使用以下命令将 Parson JSON 分析器克隆到 Raspberry Pi：

    ```sh
    cd ~
    git clone https://github.com/kgabis/parson.git
    ```

## <a name="create-a-project"></a>创建一个项目

使用与 Raspberry Pi 的 `ssh` 连接完成以下步骤：

1. 在 Raspberry Pi 上的主文件夹中创建名为 `remote_monitoring` 的文件夹。 在命令行中导航到此文件夹：

    ```sh
    cd ~
    mkdir remote_monitoring
    cd remote_monitoring
    ```

1. 在 `remote_monitoring` 文件夹中创建四个文件：`main.c`、`remote_monitoring.c`、`remote_monitoring.h` 和 `CMakeLists.txt`。

1. 在 `remote_monitoring` 文件夹中创建名为 `parson` 的文件夹。

1. 将文件 `parson.c` 和 `parson.h` 从 Parson 存储库的本地副本复制到 `remote_monitoring/parson` 文件夹。

1. 在文本编辑器中打开 `remote_monitoring.c` 文件。 在 Raspberry Pi 上，可以使用 `nano` 或 `vi` 文本编辑器。 添加以下 `#include` 语句：

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"
    ```

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="add-code-to-run-the-app"></a>添加代码以运行应用

在文本编辑器中打开 `remote_monitoring.h` 文件。 添加以下代码：

```c
void remote_monitoring_run(void);
```

在文本编辑器中打开 `main.c` 文件。 添加以下代码：

```c
#include "remote_monitoring.h"

int main(void)
{
  remote_monitoring_run();

  return 0;
}
```

## <a name="build-and-run-the-application"></a>构建并运行应用程序

以下步骤描述如何使用 *CMake* 生成客户端应用程序。

1. 在文本编辑器中，打开 `remote_monitoring` 文件夹中的 **CMakeLists.txt** 文件。

1. 添加以下指令，以定义如何生成客户端应用程序：

    ```cmake
    macro(compileAsC99)
      if (CMAKE_VERSION VERSION_LESS "3.1")
        if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
          set (CMAKE_C_FLAGS "--std=c99 ${CMAKE_C_FLAGS}")
          set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
        endif()
      else()
        set (CMAKE_C_STANDARD 99)
        set (CMAKE_CXX_STANDARD 11)
      endif()
    endmacro(compileAsC99)

    cmake_minimum_required(VERSION 2.8.11)
    compileAsC99()

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}/parson" "/usr/include/azureiot" "/usr/include/azureiot/inc")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./parson/parson.c
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./parson/parson.h
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_mqtt_transport
        aziotsharedutil
        umqtt
        pthread
        curl
        ssl
        crypto
        m
    )
    ```

1. 在 `remote_monitoring` 文件夹中，创建一个文件夹以存储 CMake 生成的 *make* 文件。 然后运行 **cmake** 和 **make** 命令，如下所示：

    ```sh
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. 运行客户端应用程序，并将遥测数据发送到 IoT 中心：

    ```sh
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
