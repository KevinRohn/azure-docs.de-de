---
title: Hinzufügen von Analysen in der Bing-Websuche-API
titleSuffix: Azure Cognitive Services
description: Die Bing-Statistik stellt Analysen für die Bing-Bildersuche-API bereit. Die Analysen enthalten das Aufrufvolumen, die häufigsten Abfragezeichenfolgen, die geografische Verteilung und vieles mehr.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: 5d1b0b19523eb37aa83aa59b24114be9f76ffa55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "68882765"
---
# <a name="add-analytics-to-the-bing-search-apis"></a>Hinzufügen von Analysen in den Bing-Suche-APIs

Die Bing-Statistik stellt Analysen für die Bing-Suche-APIs bereit. Diese Analysen enthalten das Aufrufvolumen, die häufigsten Abfragezeichenfolgen, die geografische Verteilung und vieles mehr. Sie können die Bing-Statistik im [Azure-Portal](https://ms.portal.azure.com) aktivieren. Navigieren Sie dazu zu Ihrer Azure-Ressource, und klicken Sie auf **Enable Bing Statistics** (Bing-Statistik aktivieren).

> [!IMPORTANT]
> * Für kostenlose Testabonnements oder Ressourcen im kostenlosen Tarif `F0` ist die Bing-Statistik nicht verfügbar.
> * Sie können die Daten, die über das Dashboard der Bing-Statistik verfügbar sind, nicht verwenden, um Anwendungen zu erstellen, die an Dritte verteilt werden sollen.
> * Durch das Aktivieren der Bing-Statistik wird Ihre Abonnementrate leicht erhöht. Weitere Informationen finden Sie unter [Preise](https://aka.ms/bingstatisticspricing).


In der folgenden Abbildung sind die verfügbaren Analysen für die einzelnen Bing-Suche-API-Endpunkte dargestellt.

![Verteilung nach Endpunkt (Unterstützungsmatrix)](./media/bing-statistics/bing-statistics-matrix.png)

## <a name="access-your-analytics"></a>Zugreifen auf Ihre Analysen

Bing aktualisiert die Analysedaten alle 24 Stunden und speichert den Verlauf für bis zu 13 Monate. Sie können darauf über das [Analysedashboard](https://bingapistatistics.com) zugreifen. Stellen Sie sicher, dass Sie mit dem gleichen Microsoft-Konto (MSA) angemeldet sind, das Sie für die Registrierung für die Bing-Statistik verwendet haben.

> [!NOTE]  
> * Es kann bis zu 24 Stunden dauern, bis die Metriken auf dem Dashboard angezeigt werden. Das Dashboard zeigt das Datum und die Uhrzeit des letzten Updates der Daten an.  
> * Metriken sind verfügbar, sobald Sie das Add-In „Bing-Statistik“ aktivieren.

## <a name="filter-the-data"></a>Filtern der Daten

Standardmäßig werden in den Diagrammen und Graphen alle Metriken und Daten angezeigt, auf die Sie Zugriff haben. Sie können die Daten in den Diagrammen und Graphen filtern, indem Sie die Ressourcen, Märkte, Endpunkte und Berichtszeiträume auswählen, an denen Sie interessiert sind. Sie können die folgenden Filter ändern:

- **Ressourcen-ID:** Die eindeutige Ressourcen-ID, die Ihr Azure-Abonnement identifiziert. Die Liste enthält mehrere IDs, wenn Sie ein Abonnement für mehr als einen Tarif der Bing-Suche-API besitzen. Standardmäßig sind alle Ressourcen ausgewählt.  
  
- **Märkte:** Die Märkte, aus denen die Ergebnisse stammen, z.B. en-US (Englisch, USA). Standardmäßig sind alle Märkte ausgewählt. `en-WW` ist der Markt, den Bing verwendet, wenn der Aufruf keinen Markt angibt und Bing den Markt des Benutzers nicht bestimmen kann.  
  
- **Endpunkte:** Die Endpunkte der Bing-Suche-API. Die Liste enthält alle Endpunkte, für die Sie ein kostenpflichtiges Abonnement besitzen. Standardmäßig sind alle Endpunkte ausgewählt.  

- **Zeitrahmen:** Der Berichtszeitraum. Sie können Folgendes angeben:
  - **All**: Enthält Daten aus bis zu 13 Monaten.  
  - **Letzte 24 Stunden**: Enthält Analysen der letzten 24 Stunden.  
  - **Letzte Woche**: Enthält Analysen der letzten sieben Tage.  
  - **Letzter Monat**: Enthält Analysen der letzten 30 Tage.  
  - **Benutzerdefinierter Datumsbereich**: Enthält Analysen des angegebenen Datenbereichs (falls verfügbar).  

## <a name="charts-and-graphs"></a>Diagramme und Graphen

Das Dashboard zeigt Diagramme und Graphen der Metriken an, die für den ausgewählten Endpunkt verfügbar sind. Nicht alle Metriken sind für alle Endpunkte verfügbar. Die Diagramme und Graphen für jeden Endpunkt sind statisch (d.h. Sie können nicht auswählen, welche Diagramme und Graphen angezeigt werden). Das Dashboard zeigt nur Diagramme und Graphen, für die Daten vorhanden sind.

<!--
For example, if you don't include the User-Agent header in your calls, the dashboard will not include device-related graphs.
-->

Nachfolgend sind mögliche Einschränkungen für Metriken und Endpunkte aufgeführt:

- **Aufrufvolumen:** Zeigt die Anzahl der Aufrufe auf, die während des Berichtszeitraums durchgeführt wurden. Wenn der Berichtszeitraum einen Tag beträgt, zeigt das Diagramm die Anzahl der Aufrufe pro Stunde. Andernfalls zeigt das Diagramm die Anzahl der Aufrufe pro Tag für den Berichtszeitraum.  
  
  > [!NOTE]
  > Das Aufrufvolumen kann sich von den Abrechnungsberichten unterscheiden, die in der Regel nur erfolgreiche Aufrufe beinhalten.

- **Wichtigste Abfragen:** Zeigt die häufigsten Abfragen und die Anzahl des Vorkommens jeder Abfrage während des Berichtszeitraums an. Sie können die Anzahl der angezeigten Abfragen konfigurieren. Sie können beispielsweise die 25, 50 oder 75 häufigsten Abfragen anzeigen lassen. Diese Option ist für folgende Endpunkte nicht verfügbar:  

  - /images/trending
  - /images/details
  - /images/visualsearch
  - /videos/trending
  - /videos/details
  - /news
  - /news/trendingtopics
  - /suggestions  
  
  > [!NOTE]  
  > Einige Abfrageausdrücke werden unterdrückt, um vertrauliche Informationen wie z.B. E-Mail-Adressen, Telefonnummern, SSN zu entfernen.

- **Geografische Verteilung:** Die Märkte, aus denen Suchergebnisse stammen. Beispiel: `en-us` (Englisch, USA). Bing verwendet den Abfrageparameter `mkt`, um den Markt zu bestimmen (falls angegeben). Andernfalls verwendet Bing Signale wie die IP-Adresse des Aufrufers, um den Markt zu bestimmen.

- **Verteilung von Antwortcodes:** Die HTTP-Statuscodes aller Aufrufe während des Berichtszeitraums.

- **Verteilung des Aufrufursprungs:** Die verschiedenen Browser, die von den Benutzern verwendet werden, z.B. Microsoft Edge, Chrome, Safari und Firefox. Aufrufe, die von außerhalb eines Browsers durchgeführt werden (z. B. von Bots, Postman oder „curl“ über eine Konsolen-App), werden unter „Bibliotheken“ gruppiert. Der Ursprung wird mithilfe des Werts ermittelt, den der Header „User-Agent“ der Anforderung aufweist. Wenn die Anforderung den Header „User-Agent“ nicht enthält, versucht Bing, den Ursprung aus anderen Signalen abzuleiten.  

- **Verteilung der sicheren Suche:** Die Verteilung der Werte für sicheres Suchen, z.B. „Deaktiviert“, „Mittel“ oder „Streng“. Der Abfrageparameter `safeSearch` enthält den Wert (falls angegeben). Andernfalls legt Bing den Wert standardmäßig auf „Mittel“ fest.  

- **Verteilung der angeforderten Antworten:** Die Antworten der Websuche-API, die Sie im Abfrageparameter `responseFilter` angefordert haben.  

- **Verteilung der zurückgegebenen Antworten:** Die Antworten, die die Websuche-API zurückgegeben hat.

- **Verteilung der Antwortserver:** Der Anwendungsserver, der Ihre API-Anforderungen verarbeitet hat. Die möglichen Werte sind „Bing.com“ (für Datenverkehr von Desktopgeräten und Laptops) und „Bing.com-mobile“ (für Datenverkehr von mobilen Geräten). Der Server wird mithilfe des Werts ermittelt, den der Header „User-Agent“ der Anforderung aufweist. Wenn die Anforderung den Header „User-Agent“ nicht enthält, versucht Bing, den Server aus anderen Signalen abzuleiten.

## <a name="next-steps"></a>Nächste Schritte

* [Was sind die Bing-Suche-APIs?](bing-api-comparison.md)
* [Anforderungen für die Verwendung und Anzeige der Bing-Suche-API](use-display-requirements.md)
