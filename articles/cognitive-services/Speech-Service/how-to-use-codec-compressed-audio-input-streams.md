---
title: Streamen von per Codec komprimierten Audiodaten mit dem Speech SDK – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Speech SDK komprimierte Audiodaten an den Speech-Dienst streamen. Verfügbar für C++, C# und Java für Linux, Java in Android und Objective-C in iOS.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: amishu
zone_pivot_groups: programming-languages-set-twelve
ms.openlocfilehash: 13cb35ffa650661da2855787279c4bdc37126ac9
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585017"
---
# <a name="use-codec-compressed-audio-input-with-the-speech-sdk"></a>Verwenden von per Codec komprimierter Audioeingabe mit dem Speech SDK

Die Speech Service SDK-API für **komprimierte Audioeingabestreams** ermöglicht das Streamen komprimierter Audiodaten an den Speech-Dienst mittels `PullStream` oder `PushStream`.

Das Streamen komprimierter Eingabeaudiodaten wird derzeit für C#, C++ und Java unter Windows (keine Unterstützung von UWP-Anwendungen) sowie Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9, RHEL 7/8, CentOS 7/8) unterstützt. Es wird auch für Java in Android sowie für Objective-C in iOS unterstützt.
* Das Speech SDK, Version 1.10.0 oder höher ist für RHEL 8 und CentOS 8 erforderlich.
* Das Speech SDK, Version 1.11.0 oder höher ist für Windows erforderlich.

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

## <a name="prerequisites"></a>Voraussetzungen

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-objectivec"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/objectivec/prerequisites.md)]
::: zone-end

## <a name="example-code-using-codec-compressed-audio-input"></a>Beispielcode für die Verwendung von per Codec komprimierter Audioeingabe

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/examples.md)]
::: zone-end

::: zone pivot="programming-language-objectivec"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/objectivec/examples.md)]
::: zone-end

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erkennen von Spracheingaben per Mikrofon](quickstarts/speech-to-text-from-microphone.md)
