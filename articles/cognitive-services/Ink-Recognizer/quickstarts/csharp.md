---
title: 'Schnellstart: Erkennen von Freihandeingaben mit der Freihanderkennungs-REST-API und C#'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart erfahren Sie, wie Sie die Freihanderkennungs-REST-API verwenden, um Freihandstriche zu erkennen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: aahi
ms.openlocfilehash: c24d055f1904453d2f512a278f00e23c6fea1d9b
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "80371374"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-c"></a>Schnellstart: Erkennen von Freihandeingaben mit der Freihanderkennungs-REST-API und C#

In diesem Schnellstart werden die ersten Schritte zum Senden von Freihandstrichen an die Freihanderkennungs-API erläutert. Diese C#-Anwendung sendet eine API-Anforderung, die JSON-formatierte Freihandstrichdaten enthält, und erhält die Antwort.

Die Anwendung ist zwar in C# geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist.

Normalerweise würden Sie die API über eine App für Freihandeingabe aufrufen. In diesem Schnellstart werden Freihandstrichdaten für das folgende handschriftliche Beispiel aus einer JSON-Datei gesendet.

![Abbildung, die handschriftlichen Text zeigt](../media/handwriting-sample.jpg)

Den Quellcode für diese Schnellstartanleitung finden Sie auf [GitHub](https://go.microsoft.com/fwlink/?linkid=2089502).

## <a name="prerequisites"></a>Voraussetzungen

- Eine beliebige [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)-Edition.
- [Newtonsoft.Json](https://www.newtonsoft.com/json)
    - So installieren Sie Newtonsoft.Json als NuGet-Paket in Visual Studio
        1. Klicken Sie im **Projektmappen-Manager** mit der rechten Maustaste.
        2. Klicken Sie auf **Manage NuGet Packages**
        3. Suchen Sie nach `Newtonsoft.Json`, und installieren Sie das Paket.
- Unter Linux/MacOS kann diese Anwendung mit [Mono](https://www.mono-project.com/) ausgeführt werden.

- Die Beispiel-Freihandstrichdaten für diesen Schnellstart finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Vision/InkRecognition/quickstart/example-ink-strokes.json).

### <a name="create-an-ink-recognizer-resource"></a>Erstellen einer Freihanderkennungsressource

[!INCLUDE [creating-an-ink-recognizer-resource](../includes/setup-instructions.md)]

## <a name="create-a-new-application"></a>Erstellen einer neuen Anwendung

1. Erstellen Sie in Visual Studio eine neue Konsolenprojektmappe, und fügen Sie die folgenden Pakete hinzu. 
    
    [!code-csharp[import statements](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=imports)]

2. Erstellen Sie Variablen für den Abonnementschlüssel und -endpunkt und die JSON-Beispieldatei. Der Endpunkt wird später für den Zugriff auf die API mit `inkRecognitionUrl` kombiniert. 

    [!code-csharp[endpoint file and key variables](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=vars)]

## <a name="create-a-function-to-send-requests"></a>Erstellen einer Funktion zum Senden von Anforderungen

1. Erstellen Sie eine neuen asynchronen Funktion namens `Request`, die die oben erstellten Variablen verwendet.

2. Legen Sie das Sicherheitsprotokoll und die Headerinformationen des Clients mithilfe eines `HttpClient`-Objekts fest. Achten Sie darauf, dass Sie dem Header `Ocp-Apim-Subscription-Key` Ihren Abonnementschlüssel hinzufügen. Erstellen Sie dann ein `StringContent`-Objekt für die Anforderung.
 
3. Senden Sie die Anforderung mit `PutAsync()`. Wenn die Anforderung erfolgreich ist, geben Sie die Antwort zurück.  
    
    [!code-csharp[request example method](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=request)]

## <a name="send-an-ink-recognition-request"></a>Senden einer Freihanderkennungsanforderung

1. Erstellen Sie eine neue Funktion namens `recognizeInk()`. Erstellen Sie die Anforderung und senden Sie sie, indem Sie die Funktion `Request()` mit Ihrem Endpunkt, Ihrem Abonnementschlüssel, der URL für die API und den digitalen Freihandstrichdaten aufrufen.

2. Deserialisieren Sie das JSON-Objekt, und schreiben Sie es in die Konsole. 
    
    [!code-csharp[request to recognize ink data](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=recognize)]

## <a name="load-your-digital-ink-data"></a>Laden der digitalen Freihanddaten

Erstellen Sie eine Funktion namens `LoadJson()`, um die Freihanddaten-JSON-Datei zu laden. Verwenden Sie ein `StreamReader`- und ein `JsonTextReader`-Element zum Erstellen eines `JObject`-Objekts, und geben Sie es zurück.

[!code-csharp[load the JSON file](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=loadJson)]

## <a name="send-the-api-request"></a>Senden der API-Anforderung

1. Laden Sie in der Hauptmethode Ihrer Anwendung Ihre JSON-Daten mit der oben erstellten Funktion. 

2. Rufen Sie die oben erstellte `recognizeInk()`-Funktion auf. Verwenden Sie `System.Console.ReadKey()`, damit das Konsolenfenster nach dem Ausführen der Anwendung geöffnet bleibt.
    
    [!code-csharp[file main method](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=main)]


## <a name="run-the-application-and-view-the-response"></a>Ausführen der Anwendung und Anzeigen der Antwort

Führen Sie die Anwendung aus. Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Sie finden die JSON-Antwort auch auf [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Vision/InkRecognition/quickstart/example-response.json).


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz](https://go.microsoft.com/fwlink/?linkid=2089907)


Sehen Sie sich in den folgenden Beispielanwendungen auf GitHub an, wie die Freihanderkennungs-API in einer Freihandschriftinhalte-App funktioniert:
* [C# und universelle Windows-Plattform (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# und Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript-Webbrowser-App](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Mobile Java- und Android-App](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Mobile Swift- und iOS-App](https://go.microsoft.com/fwlink/?linkid=2089805)
