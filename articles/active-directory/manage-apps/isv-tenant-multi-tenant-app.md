---
title: Erstellen eines Azure-Mandanten für eine mehrinstanzenfähige Anwendung
description: Leitfaden für unabhängige Softwarehersteller zur Integration in Azure Active Directory
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 637adba89445e6974e83486f0641576225ccd268
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "70812613"
---
# <a name="create-an-azure-tenant-for-a-multi-tenant-application"></a>Erstellen eines Azure-Mandanten für eine mehrinstanzenfähige Anwendung  

Wenn Sie Zugriff auf Ihre mehrinstanzenfähige Anwendung bereitstellen möchten, müssen Sie einen Azure Active Directory-Mandanten erstellen, um die Anwendung zu registrieren und den Verbund der Identitäten Ihres Kunden zu ermöglichen. Informationen dazu finden Sie unter [Auswählen des richtigen Verbundprotokolls für Ihre mehrinstanzenfähige Anwendung](isv-choose-multi-tenant-federation.md). Mit diesem Mandanten können Sie Ihre Anwendung und den Verbund in einer Umgebung testen, die den Azure AD-Umgebungen Ihrer Kunden ähnelt.

## <a name="costs-of-hosting-a-multi-tenant-application"></a>Kosten für das Hosten einer mehrinstanzenfähigen Anwendung

Azure Active Directory ist in mehreren Editionen verfügbar. [Hier finden Sie einen detaillierten Featurevergleich](https://azure.microsoft.com/pricing/details/active-directory/).

Sie können Ihr Azure-Abonnement und Ihre Azure Active Directory-Instanz kostenlos erstellen und Features des Basic-Tarifs nutzen.

## <a name="create-your-tenant"></a>Erstellen Ihres Mandanten

1. Erstellen Sie Ihren Mandanten. Informationen dazu finden Sie unter [Einrichten einer Entwicklungsumgebung](../develop/quickstart-create-new-tenant.md).

2. Aktivieren und testen Sie den Zugriff per einmaligem Anmelden auf Ihre Anwendung.

   a. Im Fall von **OIDC- oder OATH-Anwendungen**[registrieren Sie Ihre Anwendung](../develop/quickstart-register-app.md) als mehrinstanzenfähige Anwendung. ‎Wählen Sie unter „Unterstützte Kontotypen“ die Option „Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten“ aus.

   b. Im Fall von **SAML- und WS-Fed-basierten Anwendungen** verwenden Sie eine generische SAML-Vorlage in Azure AD, um Anwendungen für das [SAML-basierte einmalige Anmelden zu konfigurieren](configure-single-sign-on-non-gallery-applications.md).

Sie können bei Bedarf auch [eine Einzelmandantenanwendung in eine mehrinstanzenfähige Anwendung konvertieren](../develop/howto-convert-app-to-be-multi-tenant.md).

## <a name="next-steps"></a>Nächste Schritte

[Integrieren von SSO in Ihre Anwendung](isv-sso-content.md)
