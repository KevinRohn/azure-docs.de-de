---
title: Integrieren von Azure Stream Analytics mit Azure Machine Learning
description: In diesem Artikel wird beschrieben, wie Sie einen Azure Stream Analytics-Auftrag mit Azure Machine Learning-Modulen integrieren.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/19/2020
ms.openlocfilehash: 07fa72f086b676723279ee4b8efd927beb2692f0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81481972"
---
# <a name="integrate-azure-stream-analytics-with-azure-machine-learning-preview"></a>Integrieren von Azure Stream Analytics mit Azure Machine Learning (Vorschau)

Sie können Machine Learning-Modelle als benutzerdefinierte Funktionen (User-Defined Function, UDF) in Ihre Azure Stream Analytics-Aufträge implementieren, um Echtzeitbewertungen und Vorhersagen für Ihre Eingabedatenströme zu erstellen. In [Azure Machine Learning](../machine-learning/overview-what-is-azure-ml.md) können Sie mit allen beliebten Open-Source-Tools, z. B. TensorFlow, scikit-learn und PyTorch, Modelle vorbereiten, trainieren und bereitstellen.

> [!NOTE]
> Diese Funktion befindet sich in der öffentlichen Vorschauphase. Sie können auf dieses Feature im Azure-Portal nur über den [Link zur Vorschauversion des Stream Analytics-Portals](https://aka.ms/asaportalpreview) zugreifen. Diese Funktion ist auch in der neuesten Version der [Stream Analytics-Tools für Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-install) enthalten.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie ein Machine Learning-Modell als Funktion zu Ihrem Stream Analytics-Auftrag hinzufügen:

1. Verwenden Sie Azure Machine Learning, um [Ihr Modell als Webdienst bereitzustellen](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where).

2. Ihr Bewertungsskript sollte über [Beispieleingaben und -ausgaben](../machine-learning/how-to-deploy-and-where.md#example-entry-script) verfügen, mit denen Azure Machine Learning eine Schemaspezifikation generiert. Stream Analytics verwendet das Schema, um die Funktionssignatur Ihres Webdiensts zu verstehen.

3. Stellen Sie sicher, dass Ihr Webdienst serialisierte JSON-Daten akzeptiert und zurückgibt.

4. Stellen Sie Ihr Modell für umfangreiche Produktionsbereitstellungen in [Azure Kubernetes Service](../machine-learning/how-to-deploy-and-where.md#choose-a-compute-target) bereit. Wenn der Webdienst die Menge der von Ihrem Auftrag ausgehenden Anforderungen nicht verarbeiten kann, beeinträchtigt dies die Leistung Ihres Stream Analytics-Auftrags, was sich wiederum auf die Latenz auswirkt. In Azure Container Instances bereitgestellte Modelle werden zurzeit nicht unterstützt, dies wird aber in den kommenden Monaten verfügbar sein.

## <a name="add-a-machine-learning-model-to-your-job"></a>Hinzufügen eines Machine Learning-Modells zu Ihrem Auftrag

Sie können Azure Machine Learning-Funktionen direkt im Azure-Portal zu Ihrem Stream Analytics-Auftrag hinzufügen.

1. Navigieren Sie im Azure-Portal zu Ihrem Stream Analytics-Auftrag, und klicken Sie unter **Auftragstopologie** auf **Funktionen**. Wählen Sie anschließend im Dropdownmenü **+ Hinzufügen** die Option **Azure ML Service** aus.

   ![Hinzufügen einer benutzerdefinierten Azure ML-Funktion](./media/machine-learning-udf/add-azureml-udf.png)

2. Füllen Sie das Formular **Azure Machine Learning Service function** (Azure Machine Learning Service-Funktion) mit den folgenden Eigenschaftswerten aus:

   ![Konfigurieren einer benutzerdefinierten Azure ML-Funktion](./media/machine-learning-udf/configure-azureml-udf.png)

In der folgenden Tabelle werden alle Eigenschaften von Azure ML Service-Funktionen in Stream Analytics beschrieben.

|Eigenschaft|BESCHREIBUNG|
|--------|-----------|
|Funktionsalias|Geben Sie einen Namen ein, um die Funktion in der Abfrage aufzurufen.|
|Subscription|Ihr Azure-Abonnement|
|Azure ML-Arbeitsbereich|Der Azure Machine Learning-Arbeitsbereich, mit dem Sie Ihr Modell als Webdienst bereitgestellt haben|
|Bereitstellungen|Der Webdienst, der Ihr Modell hostet|
|Funktionssignatur|Hierbei handelt es sich um die Signatur Ihres Webdiensts, die aus der Schemaspezifikation der API abgeleitet wurde. Wenn beim Laden der Signatur ein Fehler auftritt, überprüfen Sie, ob Sie in Ihrem Bewertungsskript Beispieleingaben und -ausgaben für das automatische Generieren des Schemas bereitgestellt haben.|
|Number of parallel requests per partitions (Anzahl von parallelen Anforderungen pro Partition)|Hierbei handelt es sich um eine erweiterte Konfiguration zur Optimierung von umfangreichem Durchsatz. Diese Anzahl steht für die gleichzeitig von jeder Partition Ihres Auftrags an den Webdienst gesendeten Anforderungen. Aufträge mit sechs Streamingeinheiten und weniger verfügen über eine Partition. Aufträge mit 12 Streamingeinheiten verfügen über zwei Partitionen, solche mit 18 Streamingeinheiten über drei Partitionen – und so weiter.<br><br> Wenn Ihr Auftrag beispielsweise über zwei Partitionen verfügt und Sie für diesen Parameter 4 festlegen, werden acht Anforderungen von Ihrem Auftrag an Ihren Webdienst gleichzeitig ausgeführt. Während dieser öffentlichen Vorschauphase ist für diesen Wert standardmäßig 20 festgelegt, was nicht geändert werden kann.|
|Max Batch Count|Hierbei handelt es sich um eine erweiterte Konfiguration zur Optimierung von umfangreichem Durchsatz. Diese Anzahl steht für die maximale Anzahl von Ereignissen, die in einer einzelnen Anforderung zusammengefasst und an Ihren Webdienst gesendet werden können.|

## <a name="supported-input-parameters"></a>Unterstützte Eingabeparameter

Wenn Ihre Stream Analytics-Abfrage eine benutzerdefinierte Azure Machine Learning-Funktion aufruft, erstellt der Auftrag eine serialisierte JSON-Anforderung an den Webdienst. Diese Anforderung basiert auf einem modellspezifischen Schema. Sie müssen in Ihrem Bewertungsskript eine Beispieleingabe und -ausgabe bereitstellen, damit [automatisch ein Schema generiert wird](../machine-learning/how-to-deploy-and-where.md). Das Schema ermöglicht es Stream Analytics, die serialisierte JSON-Anforderung für alle unterstützten Datentypen zu erstellen, z. B. für NumPy, Pandas und PySpark. Mehrere Eingabeereignisse können in einer einzelnen Anforderung zusammengefasst werden.

Die folgende Stream Analytics-Abfrage ist ein Beispiel für das Aufrufen einer benutzerdefinierten Azure Machine Learning-Funktion:

```SQL
SELECT udf.score(<model-specific-data-structure>)
INTO output
FROM input
```

Stream Analytics unterstützt bei Azure Machine Learning-Funktionen nur die Übergabe eines Parameters. Sie müssen Ihre Daten möglicherweise vorbereiten, bevor Sie sie als Eingabe an eine benutzerdefinierte Machine Learning-Funktion übergeben können.

## <a name="pass-multiple-input-parameters-to-the-udf"></a>Übergeben mehrerer Eingabeparameter an die benutzerdefinierte Funktion

Die häufigsten Beispiele für Eingaben bei Machine Learning-Modellen sind NumPy-Arrays und -Datenrahmen. Arrays können Sie mithilfe einer benutzerdefinierten JavaScript-Funktion erstellen und serialisierte JSON-Datenrahmen mit der Klausel `WITH`.

### <a name="create-an-input-array"></a>Erstellen eines Eingabearrays

Sie können eine benutzerdefinierte JavaScript-Funktion erstellen, die *N* Eingaben akzeptiert und ein Array erstellt, das als Eingabe für Ihre benutzerdefinierte Azure Machine Learning-Funktion verwendet werden kann.

```javascript
function createArray(vendorid, weekday, pickuphour, passenger, distance) {
    'use strict';
    var array = [vendorid, weekday, pickuphour, passenger, distance]
    return array;
}
```

Sobald Sie die benutzerdefinierte JavaScript-Funktion zu Ihrem Auftrag hinzugefügt haben, können Sie Ihre benutzerdefinierte Azure Machine Learning-Funktion mithilfe der folgenden Abfrage aufrufen:

```SQL
SELECT udf.score(
udf.createArray(vendorid, weekday, pickuphour, passenger, distance)
)
INTO output
FROM input
```

Der folgende JSON-Code ist eine Beispielanforderung:

```JSON
{
    "data": [
        ["1","Mon","12","1","5.8"],
        ["2","Wed","10","2","10"]
    ]
}
```

### <a name="create-a-pandas-or-pyspark-dataframe"></a>Erstellen eines Pandas- oder PySpark-Datenrahmens

Mit der Klausel `WITH` können Sie einen serialisierten JSON-Datenrahmen erstellen, der, wie unten dargestellt, als Eingabe an Ihre benutzerdefinierte Azure Machine Learning-Funktion übergeben werden kann.

Mit der folgenden Abfrage wird ein Datenrahmen erstellt, indem die erforderlichen Felder ausgewählt werden. Anschließend wird dieser Datenrahmen als Eingabe für die benutzerdefinierte Azure Machine Learning-Funktion verwendet.

```SQL
WITH 
Dataframe AS (
SELECT vendorid, weekday, pickuphour, passenger, distance
FROM input
)

SELECT udf.score(Dataframe)
INTO output
FROM input
```

Der folgende JSON-Code ist eine Beispielanforderung für die obige Abfrage:

```JSON
{
    "data": [{
            "vendorid": "1",
            "weekday": "Mon",
            "pickuphour": "12",
            "passenger": "1",
            "distance": "5.8"
        }, {
            "vendorid": "2",
            "weekday": "Tue",
            "pickuphour": "10",
            "passenger": "2",
            "distance": "10"
        }
    ]
}
```

## <a name="optimize-the-performance-for-azure-machine-learning-udfs"></a>Optimieren der Leistung für benutzerdefinierte Azure Machine Learning-Funktionen

Wenn Sie Ihr Modell in Azure Kubernetes Service bereitstellen, können Sie [Ihr Modell profilen, um die Ressourcenverwendung zu ermitteln](../machine-learning/how-to-deploy-and-where.md#profilemodel). Sie können auch [AppInsights für Ihre Bereitstellungen aktivieren](../machine-learning/how-to-enable-app-insights.md), um Anforderungsraten, Antwortzeiten und Fehlerraten zu verstehen.

Bei einem Szenario mit hohem Ereignisdurchsatz müssen Sie möglicherweise die folgenden Parameter in Stream Analytics anpassen, um eine optimale Leistung mit geringer End-to-End-Latenz zu erreichen:

1. Max batch count (Maximal zulässige Batchanzahl)
2. Number of parallel requests per partitions (Anzahl von parallelen Anforderungen pro Partition)

### <a name="determine-the-right-batch-size"></a>Bestimmen der richtigen Batchgröße

Wenn Sie Ihren Webdienst bereitgestellt haben, senden Sie eine Beispielanforderung mit verschiedenen Batchgrößen. Beginnen Sie dabei mit 50, und erhöhen Sie die Größe dann nach und nach auf mehrere Hundert. Beispielsweise 200, 500, 1000, 2000 und so weiter Sie werden feststellen, dass sich ab einer bestimmten Batchgröße die Latenz der Antwort verlängert. Der Punkt, ab dem sich die Latenz der Antwort verlängert, sollte die maximal zulässige Batchanzahl für Ihren Auftrag sein.

### <a name="determine-the-number-of-parallel-requests-per-partition"></a>Bestimmen der Anzahl von parallelen Anforderungen pro Partition

Bei optimaler Skalierung sollte Ihr Stream Analytics-Auftrag mehrere Anforderungen gleichzeitig an Ihren Webdienst senden und innerhalb weniger Millisekunden eine Antwort erhalten können. Die Latenz der Antwort des Webdiensts kann sich direkt auf die Latenz und Leistung Ihres Stream Analytics-Auftrags auswirken. Wenn der Aufruf des Webdiensts aus Ihrem Auftrag lang dauert, werden Sie vermutlich eine längere Wasserzeichenverzögerung und möglicherweise auch eine höhere Anzahl von Eingabeereignissen im Rückstand feststellen.

Stellen Sie sicher, dass Ihr Azure Kubernetes Service-Cluster (AKS) mit der [richtigen Anzahl von Knoten und Replikaten](../machine-learning/how-to-deploy-azure-kubernetes-service.md#using-the-cli) bereitgestellt wurde, um eine solche Latenz zu verhindern. Es ist wichtig, dass Ihr Webdienst hochverfügbar ist und erfolgreiche Antworten zurückgibt. Wenn Ihr Auftrag von Ihrem Webdienst die Antwort „Dienst nicht verfügbar“ (503) erhält, wiederholt er den Vorgang kontinuierlich mit exponentiellem Backoff. Jede andere Antwort als eine über den Erfolg (200) und „Dienst nicht verfügbar“ (503) versetzt Ihren Auftrag in einen fehlerhaften Zustand.

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Azure Stream Analytics – benutzerdefinierte JavaScript-Funktionen](stream-analytics-javascript-user-defined-functions.md)
* [Skalieren eines Stream Analytics-Auftrags mit Azure Machine Learning Studio-Funktionen (klassisch)](stream-analytics-scale-with-machine-learning-functions.md)

