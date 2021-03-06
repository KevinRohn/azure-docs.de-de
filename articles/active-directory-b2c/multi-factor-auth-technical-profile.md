---
title: Technische Azure MFA-Profile in benutzerdefinierten Richtlinien
titleSuffix: Azure AD B2C
description: Benutzerdefinierter Richtlinienverweis für technische Profile von Azure Multi-Factor Authentication (MFA) in Azure AD B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c9ed0e329b498112feafaf21c34e85ea436cbb77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80332816"
---
# <a name="define-an-azure-mfa-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Definieren eines technischen Azure MFA-Profils in einer benutzerdefinierten Azure AD B2C-Richtlinie

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) bietet Unterstützung für die Überprüfung einer Telefonnummer mithilfe von Azure Multi-Factor Authentication (MFA). Verwenden Sie dieses technische Profil, um einen Code zu generieren und an eine Telefonnummer zu senden, und überprüfen Sie anschließend den Code. Das technische Azure MFA-Profil gibt möglicherweise auch eine Fehlermeldung zurück.  Mit dem technischen Validierungsprofil werden die vom Benutzer bereitgestellten Daten überprüft, bevor die User Journey fortgesetzt wird. Mit dem technischen Überprüfungsprofil wird eine Fehlermeldung auf einer Seite mit Selbstbestätigung angezeigt.

Dieses technische Profil hat folgende Eigenschaften:

- Es bietet keine Benutzeroberfläche für die Interaktion mit dem Benutzer. Stattdessen wird die Benutzeroberfläche von einem [ selbstbestätigten](self-asserted-technical-profile.md) technischen Profil oder einem [Anzeigesteuerelement](display-controls.md) als [ technisches Überprüfungsprofil](validation-technical-profile.md) aufgerufen.
- Verwendet den Dienst Azure MFA, um einen Code zu generieren und an eine Telefonnummer zu senden, und überprüft anschließend den Code.  
- Überprüft eine Telefonnummer per SMS-Nachrichten

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="protocol"></a>Protocol

Das **Name**-Attribut des **Protocol**-Elements muss auf `Proprietary` festgelegt werden. Das **handler**-Attribut muss den vollqualifizierten Namen der Protokollhandlerassembly, die von Azure AD B2C verwendet wird, enthalten:

```
Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

Das folgende Beispiel zeigt ein technisches Azure MFA-Profil:

```XML
<TechnicalProfile Id="AzureMfa-SendSms">
    <DisplayName>Send Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    ...
```

## <a name="send-sms"></a>Senden einer SMS

Der erste Modus dieses technischen Profils besteht darin, einen Code zu generieren und zu senden. Für diesen Modus können folgende Optionen konfiguriert werden:

### <a name="input-claims"></a>Eingabeansprüche

Das **InputClaims**-Element enthält eine Liste von Ansprüchen, die an Azure MFA gesendet werden sollen. Sie können auch den Namen Ihres Anspruchs dem im technischen MFA-Profil definierten Namen zuordnen.

| ClaimReferenceId | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| userPrincipalName | Ja | Der Bezeichner für den Benutzer, dem die Telefonnummer gehört. |
| phoneNumber | Ja | Die Telefonnummer, an die ein SMS-Code gesendet werden soll. |
| companyName | Nein |Der Unternehmensname in der SMS. Wenn nichts angegeben ist, wird der Name der Anwendung verwendet. |
| locale | Nein | Das Gebietsschema der SMS. Wenn nichts angegeben ist, wird das Browsergebietsschema des Benutzers verwendet. |

Das **InputClaimsTransformations**-Element enthält ggf. eine Sammlung von **InputClaimsTransformation**-Elementen, die zum Ändern der Eingabeansprüche oder zum Generieren neuer verwendet werden, bevor es an den Azure MFA-Dienst gesendet wird.

### <a name="output-claims"></a>Ausgabeansprüche

Der Azure MFA-Protokollanbieter gibt keine **OutputClaims**-Elemente zurück. Daher ist es nicht erforderlich, Ausgabeansprüche anzugeben. Sie können jedoch Ansprüche, die nicht vom Azure MFA-Identitätsanbieter zurückgegeben wurden, einfügen, sofern Sie das `DefaultValue`-Attribut festlegen.

Das **OutputClaimsTransformations**-Element darf eine Sammlung von **OutputClaimsTransformation**-Elementen, die zum Ändern der Ausgabeansprüche oder zum Generieren neuer verwendet werden, enthalten.

### <a name="metadata"></a>Metadaten

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| Vorgang | Ja | Muss **OneWaySMS** sein.  |

#### <a name="ui-elements"></a>Benutzeroberflächenelemente

Die folgenden Metadaten können verwendet werden, um die Fehlermeldungen zu konfigurieren, die bei einem SMS-Sendefehler angezeigt wird. Die Metadaten sollten im [selbstbestätigten](self-asserted-technical-profile.md) technischen Profil konfiguriert werden. Die Fehlermeldungen können [lokalisiert](localization-string-ids.md#azure-mfa-error-messages) werden.

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| UserMessageIfCouldntSendSms | Nein | Fehlermeldung für den Benutzer, wenn die angegebene Telefonnummer keine SMS-Nachrichten akzeptiert. |
| UserMessageIfInvalidFormat | Nein | Fehlermeldung für den Benutzer, wenn die angegebene Telefonnummer keine gültige Telefonnummer ist. |
| UserMessageIfServerError | Nein | Fehlermeldung für den Benutzer, wenn der Server einen internen Fehler festgestellt hat. |
| UserMessageIfThrottled| Nein | Fehlermeldung für den Benutzer, wenn eine Anforderung gedrosselt wurde.|

### <a name="example-send-an-sms"></a>Beispiel: Senden einer SMS

Das folgende Beispiel zeigt ein technisches Azure MFA-Profil, das zum Senden eines Code per SMS verwendet wird.

```XML
<TechnicalProfile Id="AzureMfa-SendSms">
  <DisplayName>Send Sms</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="Operation">OneWaySMS</Item>
  </Metadata>
  <InputClaimsTransformations>
    <InputClaimsTransformation ReferenceId="CombinePhoneAndCountryCode" />
    <InputClaimsTransformation ReferenceId="ConvertStringToPhoneNumber" />
  </InputClaimsTransformations>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userPrincipalName" />
    <InputClaim ClaimTypeReferenceId="fullPhoneNumber" PartnerClaimType="phoneNumber" />
  </InputClaims>
</TechnicalProfile>
```

## <a name="verify-code"></a>Code überprüfen

Der zweite Modus dieses technischen Profils besteht darin, einen Code zu überprüfen. Für diesen Modus können folgende Optionen konfiguriert werden:

### <a name="input-claims"></a>Eingabeansprüche

Das **InputClaims**-Element enthält eine Liste von Ansprüchen, die an Azure MFA gesendet werden sollen. Sie können auch den Namen Ihres Anspruchs dem im technischen MFA-Profil definierten Namen zuordnen.

| ClaimReferenceId | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- | ----------- |
| phoneNumber| Ja | Dieselbe Telefonnummer, die zuvor zum Senden eines Codes verwendet wurde. Außerdem wird sie verwendet, um eine Telefonüberprüfungssitzung zu suchen. |
| verificationCode  | Ja | Der Prüfcode, der vom zu überprüfenden Benutzer bereitgestellt wird. |

Das **InputClaimsTransformations**-Element enthält ggf. eine Sammlung von **InputClaimsTransformation**-Elementen, die zum Ändern der Eingabeansprüche oder zum Generieren neuer verwendet werden, bevor der Azure MFA-Dienst aufgerufen wird.

### <a name="output-claims"></a>Ausgabeansprüche

Der Azure MFA-Protokollanbieter gibt keine **OutputClaims**-Elemente zurück. Daher ist es nicht erforderlich, Ausgabeansprüche anzugeben. Sie können jedoch Ansprüche, die nicht vom Azure MFA-Identitätsanbieter zurückgegeben wurden, einfügen, sofern Sie das `DefaultValue`-Attribut festlegen.

Das **OutputClaimsTransformations**-Element darf eine Sammlung von **OutputClaimsTransformation**-Elementen, die zum Ändern der Ausgabeansprüche oder zum Generieren neuer verwendet werden, enthalten.

### <a name="metadata"></a>Metadaten

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| Vorgang | Ja | Muss **Überprüfen** sein. |

#### <a name="ui-elements"></a>Benutzeroberflächenelemente

Die folgenden Metadaten können verwendet werden, um die Fehlermeldungen zu konfigurieren, die bei einem Codeüberprüfungsfehler angezeigt wird. Die Metadaten sollten im [selbstbestätigten](self-asserted-technical-profile.md) technischen Profil konfiguriert werden. Die Fehlermeldungen können [lokalisiert](localization-string-ids.md#azure-mfa-error-messages) werden.

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| UserMessageIfMaxAllowedCodeRetryReached| Nein | Fehlermeldung für den Benutzer, wenn der Benutzer zu oft versucht hat, einen Prüfcode einzugeben. |
| UserMessageIfServerError | Nein | Fehlermeldung für den Benutzer, wenn der Server einen internen Fehler festgestellt hat. |
| UserMessageIfThrottled| Nein | Fehlermeldung für den Benutzer, wenn die Anforderung gedrosselt wird.|
| UserMessageIfWrongCodeEntered| Nein| Fehlermeldung für den Benutzer, wenn der zur Überprüfung eingegebene Code falsch ist.|

### <a name="example-verify-a-code"></a>Beispiel: Überprüfen eines Codes

Das folgende Beispiel zeigt ein technisches Azure MFA-Profil, das zum Überprüfen des Codes verwendet wird:

```XML
<TechnicalProfile Id="AzureMfa-VerifySms">
    <DisplayName>Verify Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="Operation">Verify</Item>
    </Metadata>
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="phoneNumber" PartnerClaimType="phoneNumber" />
        <InputClaim ClaimTypeReferenceId="verificationCode" />
    </InputClaims>
</TechnicalProfile>
```
