---
title: "双重验证和 AD FS - Azure MFA | Microsoft 文档"
description: "这是与 Azure Multi-Factor Authentication 相关的页面，介绍如何将 Azure MFA 与 AD FS 配合使用。"
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: yossib
ms.assetid: 44fbba68-6cf9-46c1-a9df-736580b68ae3
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/25/2017
ms.author: joflore
ms.openlocfilehash: cf76e3d87f27b69697b80eaf3f9f4bba808a0f5f
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Azure 多重身份验证服务器和 Active Directory 联合身份验证服务入门
<center>![云](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

如果组织已使用 AD FS 将本地 Active Directory 与 Azure Active Directory 联合，则使用 Azure 多重身份验证有两个选项。

* 使用 Azure 多重身份验证或 Active Directory 联合身份验证服务保护云资源
* 使用 Azure 多重身份验证服务器保护云和本地资源

下表总结了使用 Azure 多重身份验证和 AD FS 保护资源的验证体验

| 验证体验 - 基于浏览器的应用 | 验证体验 - 非基于浏览器的应用 |
|:--- |:--- |:--- |
| 使用 Azure 多重身份验证保护 Azure AD 资源 |<li>使用 AD FS 在本地执行第一个验证步骤。</li> <li>第二步是使用云身份验证实施基于手机的验证方法。</li> |
| 使用 Active Directory 联合身份验证服务保护 Azure AD 资源 |<li>使用 AD FS 在本地执行第一个验证步骤。</li><li>遵循声明在本地执行第二步。</li> |

有关联合用户的应用密码注意事项：

* 使用云身份验证验证应用密码，因此将绕过联合。 仅当设置应用密码时，才主动使用联合。
* 应用密码不遵循“本地客户端访问控制”设置。
* 使用应用密码时，无法使用本地身份验证日志记录功能。
* 帐户禁用/删除可能需要长达 3 小时才能实现目录同步，从而延迟了云标识中应用密码的禁用/删除。

有关设置 Azure 多重身份验证或具有 AD FS 的 Azure 多重身份验证服务器的信息，请参阅以下文章：

* [使用 Azure 多重身份验证和 AD FS 保护云资源](multi-factor-authentication-get-started-adfs-cloud.md)
* [将 Azure 多重身份验证服务器与 Windows Server 2012 R2 AD FS 配合使用来保护云和本地资源](multi-factor-authentication-get-started-adfs-w2k12.md)
* [将 Azure 多重身份验证服务器与 AD FS 2.0 配合使用来保护云和本地资源](multi-factor-authentication-get-started-adfs-adfs2.md)
