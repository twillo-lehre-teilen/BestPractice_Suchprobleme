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
> Nach diesem Kapitel sollten Sie die Vorgehensweisen von den vorgestellten Suchalgorithmen verstanden haben, sowie die einzelnen Schritte benennen und anwenden können. Außerdem sollten Sie in der Lage sein die Algorithmen schrittweise zu implementieren (**Python**).

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

Dieses Kapitel handelt von der [sequentiellen Suche](https://de.wikipedia.org/wiki/Lineare_Suche). Die Idee dieses Suchalgorithmus ist, dass zuerst das erste Elemente der Liste mit dem gesuchten Elemente verglichen wird, wenn sie übereinstimmen wird der aktuelle Index zurückgegeben. Wenn nicht wird der Schritt mit dem nächsten Element wiederholt. Sollte das gesuchte Element bis zum Ende der Folge nicht gefunden werden, war die Suche erfolglos und -1 wird zurückgegeben.

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
LIST = [4,8,-5,2,6,92,7,3,15,32,96,47,1,55,0,17]

def seqSearch(input):
  # your code goes here ...
  index = -1
  for i in range(0,len(list)):
    if input == LIST[i]:
      index = i
      break

  return index
```
<!-- data-readOnly="True"  style="display:block"-->
``` python -main.py
from SeqSearch import seqSearch, LIST

if __name__ == "__main__":
    print "Bitte geben Sie eine beliebige zu suchende Zahl ein:"
    input = input()
    index = seqSearch(input)
    if index != -1:
      print "Die Zahl {} befindet sich in der Liste \n{} \nan dem Index {}.".format(input, LIST, index)
    else:
      print "Die Zahl {} befindet sich nicht in der Liste \n{}.".format(input, LIST)
```
@LIA.eval(`["SeqSearch.py", "main.py"]`, `python -m compileall .`, `python main.pyc`)

<details class="panel">
<summary class="button">**Schritt 1:**</summary>

<p class="panel-content">
Schreiben Sie einen Codeabschnitt, so dass alle Elemente der Liste nacheinander (von links nach rechts) durchlaufen werden. Die $index$ variable soll dabei in jedem Druchlauf auf die aktuelle Durchlaufnummer gesetzt werden (im $i$-ten Durchlauf also auf $i$).

Um die Funktionalität Ihres Codes zu überprüfen, lassen Sie sich die $index$ variable in jedem Durchlauf auf der Konsole ausgeben.
</p>
</details>

<details class="panel">
<summary class="button">**Schritt 2:**</summary>

<p class="panel-content">Ergänzen Sie Ihren Code so, dass in jedem Durchlauf das jeweils betrachtete Element der Liste (im $i$-ten Durchlauf also das an $i$-ter Stelle) mit dem zu suchendem Element verglichen wird. Sind es die gleichen Elemente soll die $index$ variable auf $i$ gesetzt und die Schleife beendet werden.
</p>
</details>

<details class="panel">
<summary class="button">**Schritt 3:**</summary>

<p class="panel-content">
Jetzt soll das Programm so erweitert werden, dass bei dem Fall, dass das zu suchende Element nicht in der Liste vorkommt ein Index von *-1* zurückgegeben wird.
</p>
</details>

### Binäre Suche

<!--  style="background-color:#A6D492;" -->
> ## Ziel dieses Kapitels:
>
> Nach diesem Kapitel sollten Sie die Vorgehensweise von der Binären Suche verstanden haben, sowie die einzelnen Schritte benennen und anwenden können. Außerdem sollten Sie in der Lage sein den Algorithmus schrittweise zu implementieren.

---

<h4>Grundlegende Idee</h4>

Dieses Kapitel behandelt die [binäre Suche](https://de.wikipedia.org/wiki/Bin%C3%A4re_Suche). Wir stellen uns die Frage, wie die Suche effizienter werden könnte. Das Prinzip der binären Suche ist zuerst den mittleren Eintrag zu wählen und zu prüfen ob sich der gesuchte Wert in der linken oder rechten Hälfte der Liste befindet. Anschließend fährt man rekursiv mit der Hälfte fort, in der sich der Eintrag befindet. **Voraussetzung** für das binäre Suchverfahren ist, dass die Folge sortiert ist. Das Suchverfahren entspricht dem Entwurfsmuster von *Divide-and-Conquer*.


#### Beispiel

#### Implementierung

In diesem Kapitel werden Sie den Algorithmus selber schrittweise in Python implementieren. Der Code-Rahmen und Möglichkeiten Ihren Code zu testen sind jeweils schon gegeben, Sie müssen nur an den mit *"your code goes here ..."* gekennzeichneten Stellen ihren Code einfügen. Bei Bedarf ist es auch möglich eigene Testfälle zu schreiben.

Falls Sie Hilfe beim Einstieg in Python brauchen, finden Sie diese z.B. [hier](https://learnxinyminutes.com/docs/de-de/python-de/), falls es ganz schnell gehen muss oder [hier](https://www.python-lernen.de/), falls es etwas ausführlicher sein soll.

<!--  style="background-color:#A6D492;" -->
> ## Ziel dieses Kapitels
>
> Nach diesem Kapitel sollten Sie in der Lage sein, die Binäre Suche selbstständig zu implementieren.

##### Code

<!-- style="background-color:lightblue;" -->
> **Bedienungsanleitung des Code-Blocks:**
>
> - Zum ausführen des Codes müssen Sie den Button links unterhalb des Code-Blocks anklicken. Dadurch wird der gesamte Inhalt kompiliert und ausgeführt. Falls Sie anschließend Änderungen an Ihrem Code vornehmen, müssen Sie darauf achten den Button erneut anzuklicken, damit diese gespeichert und neu kompiliert werden.
>
>- Mithilfe der Pfeiltasten rechts unterhalb des Blocks können Sie zwischen Ihren Speicherständen vor und zurück wechseln, um ggf. Änderungen rückgängig zu machen oder ältere Zustände wiederherzustellen.

<!-- data-readOnly="false" -->
``` python
# die zu durchsuchende sortierte liste
LIST = [-5,0,1,2,3,4,6,7,8,15,17,32,47,55,92,96]

def binarySearch(input):
  # your code goes here ...
  return binarySearchRecursiv(input, 0, len(LIST)-1)

def binarySearchRecursiv(input, low, up):
  # your code goes here ...
  m = (low+up)/2
  if LIST[m] == input:
    return m
  if low == up:
    return -1
  if LIST[m] > input:
    return binarySearchRecursiv(input, low, m-1)
  return binarySearchRecursiv(input,m+1,up)
```
<!-- data-readOnly="True"  style="display:block"-->
``` python -main.py
from binarySearch import binarySearch, LIST

if __name__ == "__main__":
    print "Bitte geben Sie eine beliebige zu suchende Zahl ein:"
    input = input()
    index = binarySearch(input)
    if index != -1:
      print "Die Zahl {} befindet sich in der Liste \n{} \nan dem Index {}.".format(input, LIST, index)
    else:
      print "Die Zahl {} befindet sich nicht in der Liste \n{}.".format(input, LIST)
```
@LIA.eval(`["binarySearch.py", "main.py"]`, `python -m compileall .`, `python main.pyc`)

<details class="panel">
<summary class="button">**Schritt 1:**</summary>

<p class="panel-content">
Schreiben Sie zunächst die Funktion ***binarySearch()***. Diese bekommt die zu suchende Zahl übergeben und initialisiert die Binäre Suche. Dafür wird die Funktion ***binarySearchRecursiv()*** mit dem kleinstmöglichsten Index ***low*** und dem größtmöglichen Index ***up*** der zur durchsuchenden Liste aufgerufen.
</p>
</details>

<details class="panel">
<summary class="button">**Schritt 2:**</summary>

<p class="panel-content">
Schreiben Sie als nächstes die Funktion ***binarySearchRecursiv()***, die die zu suchende Zahl, sowie eine untere und obere Schranke des zu durchsuchenden Abschnitts der Liste übergeben bekommt. Zunächst muss die Mitte ***m*** abhängig von den übergebenen Schranken ***low*** und ***up*** berechnet werden. Anschließend wird kontrolliert, ob sich in der Mitte des Abschnitts die gesuchte Zahl befindet. Ist dies der Fall, wird der Index der Mitte ***m*** zurückgegeben, andernfalls wird die Funktion *binarySearchRecursiv()* **rekursiv** auf die Hälfte aufgerufen, in der die zu suchende Zahl zu vermuten ist.

*Hinweis:* Die zu durchsuchende Liste ist sortiert.
</p>
</details>

<details class="panel">
<summary class="button">**Schritt 3:**</summary>

<p class="panel-content" >
Erweitern Sie zum Abschluss die Funktion ***binarySearchRecursiv()*** nun noch so, dass die Rekursion abgebrochen wird, wenn die beiden übergebenen Schranken identisch sind. Tritt dieser Fall ein, war die Suche erfolglos und es wird *-1* zurückgegeben.
</p>
</details>

### Fibonacci-Suche

<!--  style="background-color:#A6D492;" -->
> ## Ziel dieses Kapitels:
>
> Nach diesem Kapitel sollten Sie die Vorgehensweise von der Fibonacci-Suche Suche verstanden haben, sowie die einzelnen Schritte benennen und anwenden können. Außerdem sollten Sie in der Lage sein den Algorithmus schrittweise zu implementieren.

---

<h4>Grundlegende Idee</h4>

Dieses Kapitel behandelt die Fibonacci Suche. Sie basiert auch auf der *Divide-and-Conquer* Strategie und ist eine alternative zur binären Suche. Die Fibonacci-Suche hat somit auch die **Voraussetzung**, dass die zu durchsuchende Folge sortiert sein muss.

Die im vorherigen Kapitel behandelte binäre Suche hat einige Nachteile. Die binäre Suche ist zwar der am häufigsten verwendete Algorithmus zur Suche in sortierten Arrays. Die Sprünge zu verschiedenen Testpositionen sind allerdings immer recht groß. Dies kann nachteilig sein, wenn das Array nicht vollständig im Speicher vorliegt (oder bei Datenträgertypen wie Kassetten). Außerdem werden neue Positionen durch Division berechnet und je nach Prozessor ist dies eine aufwändigere Operation als Addition und Subtraktion.

Anstatt wie bei der binären Suche das Array in gleich große Teile zu teilen, wird das Array bei der Fibonacci-Suche in Teilen entsprechend der Fibonacci-Zahlen geteilt. Es wird zunächst das Element an Indexposition ***m*** betrachtet, wobei ***m*** die größte Fibonaccizahl ist, die kleiner als die Arraylänge ist. Anschließend fährt man wie bei der binären Suche rekursiv mit dem entsprechenden Teilarray fort.




> ## Fibonacci Zahlen
>
> Zur Erinnerung, [die Folge der Fibonacci Zahlen](https://de.wikipedia.org/wiki/Fibonacci-Folge)  $F_{n}$ für $n \ge 0$ ist definiert durch:
>
>> - $F_0 = 0$
>> - $F_1 = 1$
>> - $F_2 = 1$
>> - $F_i = F_{i-1} + F_{i-2}$ für $i > 1$
>
> **Die Fibonacci-Reihe:**
>
> <!-- data-title="Fibonacci-Reihe" data-type="None" data-transpose="false" -->
> | **i** | **$F_i$** |
> |:-----:|:-----:|
> |   0   |   0   |
> |   1   |   1   |
> |   2   |   1   |
> |   3   |   2   |
> |   4   |   3   |
> |   5   |   5   |
> |   6   |   8   |
> |   7   |   13  |
> |   8   |   21  |
> |   9   |   34  |
> |   10  |   55  |
> |   11  |   89  |
> |   12  |  144  |
> |   13  |  233  |
> |   14  |  377  |

#### Beispiel

#### Implementierung

In diesem Kapitel werden Sie den Algorithmus selber schrittweise in Python implementieren. Der Code-Rahmen und Möglichkeiten Ihren Code zu testen sind jeweils schon gegeben, Sie müssen nur an den mit *"your code goes here ..."* gekennzeichneten Stellen ihren Code einfügen. Bei Bedarf ist es auch möglich eigene Testfälle zu schreiben.

Falls Sie Hilfe beim Einstieg in Python brauchen, finden Sie diese z.B. [hier](https://learnxinyminutes.com/docs/de-de/python-de/), falls es ganz schnell gehen muss oder [hier](https://www.python-lernen.de/), falls es etwas ausführlicher sein soll.

<!--  style="background-color:#A6D492;" -->
> ## Ziel dieses Kapitels
>
> Nach diesem Kapitel sollten Sie in der Lage sein, die Fibonacci Suche selbstständig zu implementieren.

##### Code

<!-- style="background-color:lightblue;" -->
> **Bedienungsanleitung des Code-Blocks:**
>
> - Zum ausführen des Codes müssen Sie den Button links unterhalb des Code-Blocks anklicken. Dadurch wird der gesamte Inhalt kompiliert und ausgeführt. Falls Sie anschließend Änderungen an Ihrem Code vornehmen, müssen Sie darauf achten den Button erneut anzuklicken, damit diese gespeichert und neu kompiliert werden.
>
>- Mithilfe der Pfeiltasten rechts unterhalb des Blocks können Sie zwischen Ihren Speicherständen vor und zurück wechseln, um ggf. Änderungen rückgängig zu machen oder ältere Zustände wiederherzustellen.

<!-- data-readOnly="true" -->
```python +Fibonacci-Folge
def fib(x):
  # gibt die x-te Fibonacci Zahl zurück
  if x == 0:
    return 0
  elif x == 1:
    return 1
  else:
    return fib(x-1) + fib(x-2)
```
<!-- data-readOnly="false" -->
``` python
from fibonacciFolge import fib

# die zu durchsuchende sortierte liste
LIST = [-5,0,1,2,3,4,6,7,8,15,17,32,47,55,92,96]

def fibonacciSearch(input):
  # your code goes here ...
  return fibonacciSearchRecursiv(input, 0, len(LIST)-1)

def fibonacciSearchRecursiv(input, low, up):
  # your code goes here ...
  m = 0
  while (fib(m) < up-low):
    m += 1
  if m != 0: m -= 1

  if LIST[low+fib(m)] == input:
    return low+fib(m)
  if low == up:
    return -1
  if input < LIST[low+fib(m)]:
    return fibonacciSearchRecursiv(input, low, low+fib(m)-1)
  return fibonacciSearchRecursiv(input,low+fib(m)+1,up)
```
<!-- data-readOnly="True"  style="display:block"-->
``` python -main.py
from fibonacciSearch import fibonacciSearch, LIST

if __name__ == "__main__":
    print "Bitte geben Sie eine beliebige zu suchende Zahl ein:"
    input = input()
    index = fibonacciSearch(input)
    if index != -1:
      print "Die Zahl {} befindet sich in der Liste \n{} \nan dem Index {}.".format(input, LIST, index)
    else:
      print "Die Zahl {} befindet sich nicht in der Liste \n{}.".format(input, LIST)
```
@LIA.eval(`["fibonacciFolge.py","fibonacciSearch.py", "main.py"]`, `python -m compileall .`, `python main.pyc`)

<details class="panel">
<summary class="button">**Schritt 1:**</summary>

<p class="panel-content">
Schreiben Sie zunächst die Funktion ***fibonacciSearch()***. Diese bekommt die zu suchende Zahl übergeben und initialisiert die Fibonacci Suche. Dafür wird die Funktion ***fibonacciSearchRecursiv()*** mit dem kleinstmöglichsten Index ***low*** und dem größtmöglichen Index ***up*** der zur durchsuchenden Liste aufgerufen.
</p>
</details>

<details class="panel">
<summary class="button">**Schritt 2:**</summary>

<p class="panel-content">
Schreiben Sie als nächstes die Funktion ***fibonacciSearchRecursiv()***, die die zu suchende Zahl, sowie eine untere und obere Schranke des zu durchsuchenden Abschnitts der Liste übergeben bekommt.

Berechnen Sie als erstes die relative Indexposition ***m*** abhängig von den übergebenen Schranken ***low*** und ***up***, wobei ***m*** die größte Fibonaccizahl ist, die kleiner als die aktuelle Listenlänge ist.

<i>**Hinweis:** Nutzen Sie zur Berechnung der Fibonaccizahlen die zur Verfügung gestellte Hilfsfunktion ***fib(x)***.</i>

</p>
</details>
<details class="panel">
<summary class="button">**Schritt 3:**</summary>

<p class="panel-content">
Kontrollieren Sie im nächsten Schritt, ob sich an der berechneten Position ***m*** des Listenabschnitts die gesuchte Zahl befindet. Ist dies der Fall, wird der Index der gesuchten Zahl zurückgegeben, andernfalls wird die Funktion *binarySearchRecursiv()* **rekursiv** auf die Hälfte aufgerufen, in der die zu suchende Zahl zu vermuten ist.

<i>**Hinweis:** Die Indexposition ***m*** ist relative zum aktuellen Listenabschnitt zu betrachten und nicht zu verwechseln mit der absoluten Indexposition.</i>

</p>
</details>
<details class="panel">
<summary class="button">**Schritt 4:**</summary>

<p class="panel-content" >
Erweitern Sie zum Abschluss die Funktion ***binarySearchRecursiv()*** nun noch so, dass die Rekursion abgebrochen wird, wenn die beiden übergebenen Schranken identisch sind. Tritt dieser Fall ein, war die Suche erfolglos und es wird *-1* zurückgegeben.
</p>
</details>

## Suchen in Texten

Nun behandeln wir das Suchen in Texten. Das Problem ist das Suchen eines Teilwortes in einem langen anderen Wort. Dies ist eine typische Funktion der Textverarbeitung. Nun ist eine effiziente Lösung gesucht. Das Maß der Effizienz ist hierbei die Anzahl der Vergleiche zwischen den Buchstaben der Worte. Den Vergleich von Zeichenketten nennt man String-Matching und eine nicht übereinstimmende Position nennt man Mismatch.

> ## Vorgegebene Daten
>
> - Worte als Array:
>
>   - zu durchsuchender Text
>   - gesuchtes Wort/Pattern
>
> - Wortlängen:
>
>   - Länge des zu durchsuchenden Textes
>   - Länge des gesuchten Wortes/Patterns
>
> - Das gesamte Alphabet (incl. $\epsilon$ (leerer string))

### Naiver Algorithmus zur Textsuche

Nun betrachten wir einen naiven Algorithmus zur Textsuche. Und zwar die direkte Lösung, **brute force**.

#### Beispiel

#### Implementierung

##### Code

### Knuth-Morris-Path

Auf dieser Seite behandeln wir den Algorithmus von Knuth-Morris-Pratt. Die Idee ist, dass bereits gelesene Informationen bei einem Mismatch genutzt werden.

#### Beispiel

#### Implementierung

##### Code


<!--dklaus@uni-osnabrueck.de-->
