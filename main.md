<!--
author:   Lennart Rosseburg für twillo

email:    support.twillo@tib.eu

repository: https://github.com/TorroRosso46/Suchprobleme

comment:  Eine Selbstlerneinheit mit interaktiven Aufgaben für die gängigsten Suchprobleme in der Informatik. Diese Seite ist lizenziert unter der [Lizenz CC-BY-SA (3.0)](https://creativecommons.org/licenses/by-sa/3.0/legalcode).

language: de

mode:     Textbook

version:  0.0.1

date:     25/03/2022

logo:

link:     https://cdn.jsdelivr.net/gh/TorroRosso46/Suchprobleme/custom.css

import:   https://github.com/LiaTemplates/Pyodide/blob/0.1.4/README.md
          https://github.com/LiaScript/CodeRunner/blob/master/README.md

@eval:  @LIA.eval(`["main.py"]`, `python -m compileall .`, `python main.pyc`)
-->
<!--
Pyodide:
- use @eval only for single code-blocks
- for multiple code-blocks define own @LIA.eval()
-->

# Suchprobleme

> # Lizenzhinweis
>
> Der Kurs *Suchprobleme*, von Lennart Rosseburg für twillo, ist lizenziert unter der [Lizenz CC-BY-SA (3.0)](https://creativecommons.org/licenses/by-sa/3.0/legalcode).
>
> #### Unter Nutzung von
>
> - Dem [Kapitel "Suchen"](https://de.wikiversity.org/wiki/Kurs:Algorithmen_und_Datenstrukturen/Vorlesung/Suchen) aus dem [Kurs "Algorithmen und Datenstrukturen"](https://de.wikiversity.org/wiki/Kurs:Algorithmen_und_Datenstrukturen), von Wikiversity unter der Beteiligung folgender [Autor:innen](https://de.wikiversity.org/w/index.php?title=Kurs:Algorithmen_und_Datenstrukturen/Vorlesung/Sortieren_Grundlagen&action=history), unter der [Lizenz CC-BY-SA (3.0)](https://creativecommons.org/licenses/by-sa/3.0/legalcode).
>
> - Dem [Artikel "Difference between List and Array in Python"](https://www.geeksforgeeks.org/difference-between-list-and-array-in-python/), veröffentlicht auf [geeksforgeeks](https://www.geeksforgeeks.org/) von [Yash Chuahan](https://auth.geeksforgeeks.org/user/yashchuahan/articles), unter der [Lizenz CC-BY-SA (4.0)](https://creativecommons.org/licenses/by-sa/4.0/legalcode).

<!-- style="background-color:transparent;" -->
> Diese Selbstlerneinheit konzentriert sich auf die Funktionsweise verschiedener Suchalgorithmen und enthält interaktive Aufgaben um das Gelernte zu überprüfen und zu verinnerlichen.

<!--  style="background-color:#A6D492;" -->
> #### Ziel des Kurses:
>
> Am Ende dieser Selbstlerneinheit sollten Sie die vorgestellten Suchalgorithemn unterscheiden, in den Kontext von Suchproblemen in der Informatik einordnen und selbst anwenden können.

## Einleitung

In dieser Lektion geben wir einen Überblick über das Thema Suchen. Suchprobleme sind eine der häufigsten Probleme in der Informatik. Man kann in sortierten Folgen suchen, Zeichenketten im Text suchen, Dokumente in Textkorpora suchen, oder allgemeine Lösungen von Problemräumen, wie der Spielbaumsuche oder der Plansuche, suchen. Hier behandeln wir zunächst die Suche in sortierten Folgen, gefolgt von der Suche in Texten.

## Suchen in sortierten Folgen

<!--  style="background-color:#A6D492;" -->
> ## Ziel dieses Kapitels:
>
> Nach diesem Kapitel sollten Sie die Vorgehensweisen von vorgestellten Suchalgorithmen verstanden haben, sowie die einzelnen Schritte benennen und anwenden können. Außerdem sollten Sie in der Lage sein die Algorithmen schrittweise zu implementieren (**Python**).

---

<h4>Grundlagen - lineare Datenstrukturen</h4>
<h5>Defintion</h5>

Eine lineare Datenstruktur $L$ ist eine Sequenz $L=(a_{1}...,a_{n})$. Die lineare Datenstruktur ordnet Elemente (entweder primitive Datentypen oder komplexere Datenstrukturen) in einer linearen Anordnung an.

> ## Beispiel
>
> Zahlenfolgen: $[5,4,6,1,3,2]$
>
> Strings: $[L,I,N,E,A,R]$

<h5>Arrays und Listen</h5>

Es gibt zwei Möglichkeiten lineare Datenstrukturen zu realisieren. Entweder mit Arrays oder mit Listen. Arrays belegen einen zusammenhängenden Bereich im Speicher. Elemente einer Liste können dagegen beliebig verteilt sein, brauchen dadurch allerdings mehr Platz. Ob zur Realisierung einer linearen Datenstruktur ein Array oder eine Liste verwendet wird, hängt von der Anwendung ab. Arrays werden meist für statische Datenstrukturen verwendet, d.h. wenn die Länge des Arrays nicht verändert wird. Listen werden meist für dynamische Datenstrukturen verwendet, d.h. wenn die Länge variabel ist. Innerhalb einer Liste können Elemente unterschiedliche Datentypen besitzen (Bsp. Numerisch, Logisch, etc.), im Gegensatz dazu müssen alle Elemente in einem Array den selben Datentyp haben. Zu den positiven Eigenschaften von Arrays zählen der schnelle Zugriff auf Einzelelemente durch den Index, wodurch arithmetische Operationen direkt verarbeitet werden können. Aus diesem Grunde werden Arrays den Listen in der Datananalyse bevorzugt. Der Nachteil von Arrays ist das sehr aufwändige Modifizieren von Elementen, welches elementweise durchgeführt werden muss. Hierfür sind Listen deutlich flexibler. Listen werden daher häufig bei kleineren Datensequenzen bevorzugt.

> ## Hinweis
>
> Da wir in dieser Selbstlerneinheit nur mit kleineren linearen Datenstrukturen arbeiten, bei denen der Speicherverbrauch vernachlässigbar ist, und nur einfache Operationen benötigt werden, empfehlen wir der Einfachheit halber das nutzen von Listen anstatt von Arrays für die kommenden Programmieraufgaben.


<h5>Integrierte List Operationen in Python</h5>

Schreiboperationen:

- $append(e)$: Element $e$ am Ende der Liste einfügen.
- $extend(e)$: Sammlung iterierbarer Elemente $e$ am Ende der Liste ein.
- $insert(i, e)$: Element $e$ an Position $i$ einfügen
- $pop(i)$: Element an Position $i$ entfernen
- $remove(e)$: Element $e$ entfernen
- $clear()$: alle Elemente aus der Liste entfernen
- $reverse()$: kehrt die Reihenfolge der Elemente in der Liste um
- $sort()$: sortiert die Elemente der Liste auf- oder absteigend

Leseoperationen:

- $count(e)$: gibt zurück, wie oft das Element $e$ in der Liste vorkommt
- $copy()$: gibt eine Kopie der Liste zurück
- $index(e)$: gibt den Index des Elements $e$ zurück

### Sequentielle Suche

<!--  style="background-color:#A6D492;" -->
> ## Ziel dieses Kapitels:
>
> Nach diesem Kapitel sollten Sie die Vorgehensweise von der Sequentiellen Suche verstanden haben, sowie die einzelnen Schritte benennen und anwenden können. Außerdem sollten Sie in der Lage sein den Algorithmus schrittweise zu implementieren.

---

<h4>Grundlegende Idee</h4>

Dieses Kapitel handelt von der sequentiellen Suche. Die Idee dieses Suchalgorithmus ist, dass zuerst das erste Elemente der Liste mit dem gesuchten Elemente verglichen wird, wenn sie übereinstimmen wird der aktuelle Index zurückgegeben. Wenn nicht wird der Schritt mit dem nächsten Element wiederholt. Sollte das gesuchte Element bis zum Ende der Folge nicht gefunden werden, war die Suche erfolglos und -1 wird zurückgegeben.

#### Beispiel

#### Implementierung

In diesem Kapitel werden Sie den Algorithmus selber schrittweise in Python implementieren. Der Code-Rahmen und Möglichkeiten Ihren Code zu testen sind jeweils schon gegeben, Sie müssen nur an den mit *"your code goes here ..."* gekennzeichneten Stellen ihren Code einfügen. Bei Bedarf ist es auch möglich eigene Testfälle zu schreiben.

Falls Sie Hilfe beim Einstieg in Python brauchen, finden Sie diese z.B. [hier](https://learnxinyminutes.com/docs/de-de/python-de/), falls es ganz schnell gehen muss oder [hier](https://www.python-lernen.de/), falls es etwas ausführlicher sein soll.

<!--  style="background-color:#A6D492;" -->
> ## Ziel dieses Kapitels
>
> Nach diesem Kapitel sollten Sie in der Lage sein, den sequentiellen Suchalgorithmus selbstständig zu implementieren.

##### Code

<!-- style="background-color:lightblue;" -->
> **Bedienungsanleitung des Code-Blocks:**
>
> - Zum ausführen des Codes müssen Sie den Button links unterhalb des Code-Blocks anklicken. Dadurch wird der gesamte Inhalt kompiliert und ausgeführt. Falls Sie anschließend Änderungen an Ihrem Code vornehmen, müssen Sie darauf achten den Button erneut anzuklicken, damit diese gespeichert und neu kompiliert werden.
>
>- Mithilfe der Pfeiltasten rechts unterhalb des Blocks können Sie zwischen Ihren Speicherständen vor und zurück wechseln, um ggf. Änderungen rückgängig zu machen oder ältere Zustände wiederherzustellen.

<!-- data-readOnly="false" -->
``` python
# die zu durchsuchende liste
list = [4,8,2,6,7,3,15,32,96,47,1,55,0,17]

def seqSearch(input):
  index = None
  # your code goes here ...
  for i in range(0,len(list)):
    if input == list[i]:
      index = i
      break
  if index == None:
    index = -1

  return list, input, index
```
<!-- data-readOnly="True"  style="display:block"-->
``` python -main.py
from SeqSearch import seqSearch

if __name__ == "__main__":
    print "Bitte geben Sie eine beliebige zu suchende Zahl ein:"
    input = input()
    list, number, index = seqSearch(input)
    if index != -1:
      print "Die Zahl {} befindet sich in der Liste \n{} \nan dem Index {}.".format(number, list, index)
    else:
      print "Die Zahl {} befindet sich nicht in der Liste \n{}.".format(number, list)
```
@LIA.eval(`["SeqSearch.py", "main.py"]`, `python -m compileall .`, `python main.pyc`)

<details class="panel">
<summary class="button">**Schritt 1:**</summary>

<p class="panel-content">
<i>Schreiben Sie einen Codeabschnitt, so dass alle Elemente der Liste nacheinander (von links nach rechts) durchlaufen werden. Die $index$ variable soll dabei in jedem Druchlauf auf die aktuelle Durchlaufnummer gesetzt werden (im $i$-ten Durchlauf also auf $i$).
</i>
</p>
</details>

<details class="panel">
<summary class="button">**Schritt 2:**</summary>

<p class="panel-content">
<i>Ergänzen Sie Ihren Code so, dass in jedem Durchlauf das jeweils betrachtete Element der Liste (im $i$-ten Durchlauf also das an $i$-ter Stelle) mit dem zu suchendem Element verglichen wird. Sind es die gleichen Elemente soll die $index$ variable auf $i$ gesetzt und die Schleife beendet werden.
</i>
</p>
</details>

<details class="panel">
<summary class="button">**Schritt 3:**</summary>

<p class="panel-content" >
<i>Jetzt soll das Programm so erweitert werden, dass bei dem Fall, dass das zu suchende Element nicht in der Liste vorkommt ein Index von *-1* zurückgegeben wird.
</i>
</p>
</details>

### Binäre Suche

<!--  style="background-color:#A6D492;" -->
> ## Ziel dieses Kapitels:
>
> Nach diesem Kapitel sollten Sie die Vorgehensweise von der Binären Suche verstanden haben, sowie die einzelnen Schritte benennen und anwenden können. Außerdem sollten Sie in der Lage sein den Algorithmus schrittweise zu implementieren.

---

<h4>Grundlegende Idee</h4>

#### Beispiel

#### Implementierung

### Fibonacci-Suche

<!--  style="background-color:#A6D492;" -->
> ## Ziel dieses Kapitels:
>
> Nach diesem Kapitel sollten Sie die Vorgehensweise von der Fibonacci-Suche Suche verstanden haben, sowie die einzelnen Schritte benennen und anwenden können. Außerdem sollten Sie in der Lage sein den Algorithmus schrittweise zu implementieren.

---

<h4>Grundlegende Idee</h4>

#### Beispiel

#### Implementierung

## Suchen in Texten

### Naiver Algorithmus zur Textsuche

### Knuth-Morris-Path
