---
title: Microsoft Azure Cloud Services のアプリケーションとサービスの可用性の問題についてよくあるご質問 | Microsoft Docs
description: この記事では、Microsoft Azure Cloud Services のアプリケーションとサービスの可用性についてよくあるご質問を紹介します。
services: cloud-services
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: fb4b5dde63d8c7c75419d3202d9848cd6fde8b8a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2019
ms.locfileid: "57547641"
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure Cloud Services のアプリケーションとサービスの可用性の問題:よく寄せられる質問 (FAQ)

この記事では、[Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services) のアプリケーションとサービスの可用性の問題についてよくあるご質問を紹介します。 サイズについては、 [Cloud Services VM サイズのページ](cloud-services-sizes-specs.md) を参照してください。

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a>私のロールがリサイクルされました。 私のクラウド サービスに何かの更新プログラムが展開されたのですか。
ほぼ月に 1 回、Microsoft Azure の PaaS VM に新しいゲスト OS バージョンがリリースされます。 ゲスト OS は、パイプラインにおいて唯一のこのような更新です。 リリースは、他の多くの要因の影響を受けることがあります。 さらに、Azure は、数十万台のマシンで実行されています。 このため、お使いのロールが再起動する正確な日時を予測することはできません。 ゲスト OS の更新 RSS フィードは Microsoft で把握している最新の情報に更新されていますが、伝えられている時刻はおおよその時刻であるとお考えください。 これがお客様にご迷惑をおかけしていることを念頭に置き、Microsoft では、再起動の制限や正確な時間の指定をできるように取り組んでおります。

最新のゲスト OS の更新に関する完全な詳細については、「[Azure ゲスト OS リリースと SDK の互換性対応表](cloud-services-guestos-update-matrix.md)」を参照してください。

再起動に関する有益な情報と、ゲスト OS とホスト OS の更新プログラムの技術的な詳細のヒントについては、MSDN の [OS のアップグレードによるロール インスタンスの再起動](https://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)に関するブログ記事を参照してください。

## <a name="why-does-the-first-request-to-my-cloud-service-after-the-service-has-been-idle-for-some-time-take-longer-than-usual"></a>クラウド サービスがしばらくの間アイドル状態だった後、サービスに対する最初の要求が通常より長くかかるのはなぜですか。
Web サーバーが最初の要求を受信すると、まずコードを再コンパイルしてから要求を処理します。 最初の要求にかかる時間が他の要求よりも長い理由はそこにあります。 既定では、ユーザーが非アクティブになると、アプリケーション プールがシャットダウンします。 また、アプリケーション プールは、既定では 1,740 分 (29 時間) ごとにリサイクルされます。

インターネット インフォメーション サービス (IIS) のアプリケーション プールは、アプリケーションのクラッシュ、ハング、メモリ リークの発生につながる可能性がある不安定な状態を避けるために、定期的にリサイクルすることができます。

次のドキュメントは、この問題について理解し、軽減するために役立ちます。
* [Fixing slow initial load for IIS](https://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis) (IIS の初期ロードが遅い問題を修正する)
* [IIS 7.5 web application first request after app-pool recycle very slow](https://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow) (アプリケーション プールのリサイクル後、IIS 7.5 Web アプリケーションによる最初の要求の処理が非常に低速)

IIS の既定の動作を変更する場合は、スタートアップ タスクを使用する必要があります。これは、Web ロール インスタンスに手動で変更を適用すると、その変更は最終的に失われてしまうためです。

詳細については、「[クラウド サービスのスタートアップ タスクを構成して実行する方法](cloud-services-startup-tasks.md)」を参照してください。
