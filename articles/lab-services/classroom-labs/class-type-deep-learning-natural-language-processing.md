---
title: Einrichten eines Labs mit Schwerpunkt Deep Learning mit Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Lab für Schulungen zu Shellskripts unter Linux einrichten.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2020
ms.author: spelluru
ms.openlocfilehash: 1167846c399430bd2db2eaa3114628ebb63ce639
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592320"
---
# <a name="set-up-a-lab-focused-on-deep-learning-in-natural-language-processing-using-azure-lab-services"></a>Einrichten eines Labs mit Schwerpunkt Deep Learning und Verarbeitung natürlicher Sprache mithilfe von Azure Lab Services
Dieser Artikel beschreibt, wie Sie ein Lab mit Schwerpunkt Deep Learning mit Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) mithilfe von Azure Lab Services einrichten. NLP ist eine Form der künstlichen Intelligenz (KI), die Computern Übersetzungs-, Spracherkennungs- und andere Sprachverständnisfunktionen bietet.  

Kursteilnehmer, die einen Kurs zur Verarbeitung natürlicher Sprache belegen, erhalten einen virtuellen Linux-Computer, auf dem Sie lernen, wie neuronale Netzwerkalgorithmen angewendet werden, um Deep Learning-Modelle zum Analysieren geschriebener menschlicher Sprache zu entwickeln. 

## <a name="lab-configuration"></a>Labkonfiguration
Zum Einrichten dieses Labs benötigen Sie als Einstieg ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Sobald Sie über ein Azure-Abonnement verfügen, können Sie entweder ein neues Lab-Konto in Azure Lab Services erstellen oder ein vorhandenes Lab-Konto verwenden. Sehen Sie sich das folgende Tutorial zum Erstellen eines neuen Lab-Kontos an: [Tutorial zum Einrichten eines Lab-Kontos](tutorial-setup-lab-account.md).
 
Nachdem Sie das Lab-Konto erstellt haben, aktivieren Sie die folgenden Einstellungen im Lab-Konto: 

| Lab-Kontoeinstellungen | Instructions |
| ----------- | ------------ |  
| Marketplace-Bilder | Aktivieren Sie das Image Data Science Virtual Machine für Linux (Ubuntu) zur Verwendung in Ihrem Lab-Konto.  Anweisungen finden Sie im folgenden Artikel: [Für Lab-Ersteller verfügbare Marketplace-Images festlegen](specify-marketplace-images.md). | 

Folgen Sie [diesem Tutorial](tutorial-setup-classroom-lab.md), um ein neues Lab mit den folgenden Einstellungen zu erstellen:

| Lab-Einstellungen | Wert/Anweisungen | 
| ------------ | ------------------ |
| Größe des virtuellen Computers (VM) | Kleine GPU (Compute). Diese Größe eignet sich am besten für rechenintensive und netzwerkintensive Anwendungen wie künstliche Intelligenz und Deep Learning. |
| VM-Image | [Data Science Virtual Machine für Linux (Ubuntu)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.ubuntu-1804). Dieses Image bietet Frameworks für Deep Learning sowie Tools für maschinelles Lernen und Data Science. Die vollständige Liste der auf diesem Image installierten Tools finden Sie im folgenden Artikel: [Was ist in DSVM enthalten?](../../machine-learning/data-science-virtual-machine/overview.md#whats-included-on-the-dsvm). |
| Remotedesktopverbindung aktivieren | <p>Wenn Sie diese Einstellung aktivieren, können Lehrer/Dozenten und Kursteilnehmer mit Hilfe von Remotedesktop (RDP) eine Verbindung mit ihren VMs herstellen.</p><p>**Wichtig**: Beim Aktivieren dieser Einstellung wird auf Linux-Computern nur der **RDP**-Port geöffnet. Wenn RDP bereits für das VM-Image installiert und konfiguriert ist, können Sie/Kursteilnehmer über RDP eine Verbindung mit virtuellen Computern herstellen, ohne weitere Schritte ausführen zu müssen. <p>Wenn in dem VM-Image kein RDP installiert und konfiguriert ist, müssen Sie beim ersten Mal eine Verbindung mit dem Linux-Computer per SSH herstellen und dann RDP- und GUI-Pakete installieren, damit Sie/Studenten später eine Verbindung mit dem Linux-Computer per RDP herstellen können. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Remotedesktop zum Herstellen einer Verbindung mit einem virtuellen Linux-Computer in Azure](../../virtual-machines/linux/use-remote-desktop.md). Anschließend veröffentlichen Sie das Image, damit die Kursteilnehmer per RDP eine Verbindung mit den virtuellen Linux-Computern herstellen können, die für sie bestimmt sind.  |

Das Image der Data Science Virtual Machine für Linux bietet die für Kurse dieser Art erforderlichen Deep Learning-Frameworks und -Tools. Es muss daher nach dem Erstellen des Vorlagen-Computers nicht weiter angepasst werden. Es kann zur Nutzung durch die Kursteilnehmer veröffentlicht werden. Wählen Sie auf der Vorlagenseite die Schaltfläche **Veröffentlichen** aus, um die Vorlage im Lab zu veröffentlichen.  

## <a name="cost"></a>Kosten
Das folgende Beispiel dient der Einschätzung der Lab-Kosten: 

Für einen Kurs mit 25 Teilnehmern, 20 planmäßigen Kursstunden und 10 Stunden Hausaufgaben bzw. Arbeitsaufträgen entstünden folgende Kosten für das Lab: 25 Kursteilnehmer x (20 + 10) Stunden x 139 Lab-Einheiten x 0,01 USD pro Stunde = 1042,50 USD

Weitere Informationen zu den Preisen finden Sie unter [Azure Lab Services-Preise](https://azure.microsoft.com/pricing/details/lab-services/).

## <a name="conclusion"></a>Zusammenfassung
In diesem Artikel wurden die Schritte zum Erstellen eines Labs für Kurse zur Verarbeitung natürlicher Sprachen dargelegt. Sie können ein ähnliches Setup für andere Deep Learning-Kurse verwenden.

## <a name="next-steps"></a>Nächste Schritte
Die nächsten Schritte sind die gleichen für sämtliche Labs:

- [Hinzufügen von Benutzern](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kontingent festlegen](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zeitplan festlegen](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab) 
- [Registrierungslinks an Kursteilnehmer senden](how-to-configure-student-usage.md#send-invitations-to-users). 

