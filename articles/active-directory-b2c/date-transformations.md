---
title: Beispiele für die Transformation von Datumsansprüchen für benutzerdefinierte Richtlinien
description: Beispiele für die Transformation von Datumsansprüchen für das Schema des Frameworks für die Identitätsfunktion (Identity Experience Framework, IEF) von Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/16/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c02ac9392d6f3f95deef38ff86250e96dfb76d96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476687"
---
# <a name="date-claims-transformations"></a>Transformationen von Datumsansprüchen

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dieser Artikel enthält Beispiele für die Verwendung von Transformationen von Datumsansprüchen des Identity Experience Framework-Schemas in Azure Active Directory B2C (Azure AD B2C). Weitere Informationen finden Sie unter [ClaimsTransformations](claimstransformations.md).

## <a name="assertdatetimeisgreaterthan"></a>AssertDateTimeIsGreaterThan

Überprüft, ob ein Datums- und Uhrzeitanspruch (Zeichenfolgen-Datentyp) nach einem zweiten Datums- und Uhrzeitanspruch (Zeichenfolgen-Datentyp) liegt, und löst eine Ausnahme aus.

| Element | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | leftOperand | Zeichenfolge | Typ des ersten Anspruchs, der nach dem zweiten Anspruch liegen sollte. |
| InputClaim | rightOperand | Zeichenfolge | Typ des zweiten Anspruchs, der vor dem ersten Anspruch liegen sollte. |
| InputParameter | AssertIfEqualTo | boolean | Gibt an, ob diese Assertion positiv ausfallen soll, wenn der linke Operand gleich dem rechten Operanden ist. |
| InputParameter | AssertIfRightOperandIsNotPresent | boolean | Gibt an, ob diese Assertion positiv ausfallen soll, wenn der rechte Operanden fehlt. |
| InputParameter | TreatAsEqualIfWithinMillseconds | INT | Gibt die Anzahl der Millisekunden an, die zwischen den beiden Datum/Uhrzeit-Werten liegen darf, damit die Zeiten als gleich angesehen werden (z. B. Abweichungen durch Gangungenauigkeiten bei Uhren). |

Die Anspruchstransformation **AssertDateTimeIsGreaterThan** wird immer über ein [technisches Validierungsprofil](validation-technical-profile.md) ausgeführt, das von einem [selbstbestätigten technischen Profil](self-asserted-technical-profile.md) aufgerufen wird. Die Metadaten des selbstbestätigten technischen Profils **DateTimeGreaterThan** steuern die Fehlermeldung, die das technische Profil dem Benutzer anzeigt. Die Fehlermeldungen können [lokalisiert](localization-string-ids.md#claims-transformations-error-messages) werden.

![Ausführung von AssertStringClaimsAreEqual](./media/date-transformations/assert-execution.png)

Im folgenden Beispiel wird der `currentDateTime`-Anspruch mit dem `approvedDateTime`-Anspruch verglichen. Ein Fehler wird ausgelöst, wenn `currentDateTime` nach `approvedDateTime` liegt. Die Transformation behandelt Werte als gleich, wenn sie innerhalb eines Abstands von 5 Minuten (30.000 Millisekunden) liegen.

```XML
<ClaimsTransformation Id="AssertApprovedDateTimeLaterThanCurrentDateTime" TransformationMethod="AssertDateTimeIsGreaterThan">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="approvedDateTime" TransformationClaimType="leftOperand" />
    <InputClaim ClaimTypeReferenceId="currentDateTime" TransformationClaimType="rightOperand" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="AssertIfEqualTo" DataType="boolean" Value="false" />
    <InputParameter Id="AssertIfRightOperandIsNotPresent" DataType="boolean" Value="true" />
    <InputParameter Id="TreatAsEqualIfWithinMillseconds" DataType="int" Value="300000" />
  </InputParameters>
</ClaimsTransformation>
```

Das `login-NonInteractive`technische Validierungsprofil ruft die `AssertApprovedDateTimeLaterThanCurrentDateTime`-Anspruchstransformation auf.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertApprovedDateTimeLaterThanCurrentDateTime" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Das selbstbestätigte technische Profil ruft das technische Validierungsprofil **login-NonInteractive** auf.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="DateTimeGreaterThan">Custom error message if the provided left operand is greater than the right operand.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
    - **leftOperand**: 2020-03-01T15:00:00.0000000Z
    - **rightOperand**: 2020-03-01T14:00:00.0000000Z
- Ergebnis: Fehler wird ausgelöst.

## <a name="convertdatetodatetimeclaim"></a>ConvertDateToDateTimeClaim

Konvertiert einen Anspruchstyp **Date** in einen Anspruchstyp **DateTime**. Die Anspruchstransformation konvertiert das Uhrzeitformat und fügt dem Datum 12:00:00 AM hinzu.

| Element | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | date | Der Anspruchstyp, der konvertiert werden soll. |
| OutputClaim | outputClaim | dateTime | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. |

Das folgende Beispiel veranschaulicht die Konvertierung des Anspruchs `dateOfBirth` (Datentyp „date“) in einen anderen Anspruch `dateOfBirthWithTime` (Datentyp „dateTime“).

```XML
  <ClaimsTransformation Id="ConvertToDateTime" TransformationMethod="ConvertDateToDateTimeClaim">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="dateOfBirth" TransformationClaimType="inputClaim" />
    </InputClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="dateOfBirthWithTime" TransformationClaimType="outputClaim" />
    </OutputClaims>
  </ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
    - **inputClaim:** 2020-15-03
- Ausgabeansprüche:
    - **outputClaim:** 2020-15-03T00:00:00.0000000Z

## <a name="convertdatetimetodateclaim"></a>ConvertDateTimeToDateClaim

Wandelt den Anspruchstyp **DateTime** in den Anspruchstyp **Date** um. Die Anspruchstransformation entfernt das Uhrzeitformat aus dem Datum.

| Element | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | dateTime | Der Anspruchstyp, der konvertiert werden soll. |
| OutputClaim | outputClaim | date | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. |

Das folgende Beispiel veranschaulicht die Konvertierung des Anspruchs `systemDateTime` (Datentyp „dateTime“) in einen anderen Anspruch `systemDate` (Datentyp „date“).

```XML
<ClaimsTransformation Id="ConvertToDate" TransformationMethod="ConvertDateTimeToDateClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="systemDateTime" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="systemDate" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
  - **inputClaim:** 2020-15-03T11:34:22.0000000Z
- Ausgabeansprüche:
  - **outputClaim:** 2020-15-03

## <a name="getcurrentdatetime"></a>GetCurrentDateTime

Ruft den aktuellen UTC-Datum/Uhrzeit-Wert ab und addiert den Wert zu einem Anspruchstyp.

| Element | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| OutputClaim | currentDateTime | dateTime | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. |

```XML
<ClaimsTransformation Id="GetSystemDateTime" TransformationMethod="GetCurrentDateTime">
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="systemDateTime" TransformationClaimType="currentDateTime" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

* Ausgabeansprüche:
    * **currentDateTime**: 2020-15-03T11:40:35.0000000Z

## <a name="datetimecomparison"></a>DateTimeComparison

Bestimmt, ob ein dateTime-Wert größer, kleiner oder gleich einem anderen ist. Das Ergebnis ist ein neuer boolescher Anspruchstyp mit einem Wert von `true` oder `false`.

| Element | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | firstDateTime | dateTime | Der erste dateTime-Wert, für den überprüft werden soll, ob er vor oder nach einem zweiten dateTime-Wert liegt. Ein Null-Wert löst eine Ausnahme aus. |
| InputClaim | secondDateTime | dateTime | Der zweite dateTime-Wert, für den überprüft werden soll, ob er vor oder nach dem ersten dateTime-Wert liegt. Ein NULL-Wert wird als dateTime-Wert der aktuellen Uhrzeit behandelt. |
| InputParameter | Operator | Zeichenfolge | Einer der folgenden Werte: identisch (same), später als (later than) oder früher als (earlier than). |
| InputParameter | timeSpanInSeconds | INT | Addiert den Zeitraum zum ersten Datum/Uhrzeit-Wert. |
| OutputClaim | result | boolean | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. |

Mit dieser Anspruchstransformation können Sie bestimmen, ob zwei Anspruchstypen gleich sind oder der eine vor oder nach dem anderen liegt. Sie können z. B. den letzten Zeitpunkt speichern, zu dem ein Benutzer Ihre Nutzungsbedingungen akzeptiert hat. Nach 3 Monaten können Sie den Benutzer auffordern, die Nutzungsbedingungen erneut zu akzeptieren.
Um die Anspruchstransformation auszuführen, müssen Sie zuerst den aktuellen Datum/Uhrzeit-Wert abrufen sowie den letzte Zeitpunkt, zu dem der Benutzer die Nutzungsbedingungen akzeptiert hat.

```XML
<ClaimsTransformation Id="CompareLastTOSAcceptedWithCurrentDateTime" TransformationMethod="DateTimeComparison">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="currentDateTime" TransformationClaimType="firstDateTime" />
    <InputClaim ClaimTypeReferenceId="extension_LastTOSAccepted" TransformationClaimType="secondDateTime" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="operator" DataType="string" Value="later than" />
    <InputParameter Id="timeSpanInSeconds" DataType="int" Value="7776000" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isLastTOSAcceptedGreaterThanNow" TransformationClaimType="result" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
    - **firstDateTime**: 2020-01-01T00:00:00.100000Z
    - **secondDateTime**: 2020-04-01T00:00:00.100000Z
- Eingabeparameter:
    - **operator:** später als
    - **timeSpanInSeconds**: 7776000 (90 Tage)
- Ausgabeansprüche:
    - **result**: true
