<!--
author:   Lennart Rosseburg für twillo

email:    support.twillo@tib.eu

comment:  Eine Selbstlerneinheit mit interaktiven Aufgaben für die gängigsten Suchprobleme in der Informatik. Diese Seite ist lizenziert unter der [Lizenz CC-BY-SA (3.0)](https://creativecommons.org/licenses/by-sa/3.0/legalcode).

language: de

mode:     Textbook

version:  1.0.0

date:     11/05/2022

logo:     docs/thumbnail.JPG

icon:     docs/twillo_logo.svg

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
>
> - Dem [Artikel "Knuth-Morris-Pratt-Algorithmus"](https://de.wikipedia.org/wiki/Knuth-Morris-Pratt-Algorithmus), veröffentlicht auf [Wikipedia](https://de.wikipedia.org/wiki/Wikipedia:Hauptseite) von den [Autor:innen](https://xtools.wmflabs.org/articleinfo-authorship/de.wikipedia.org/Knuth-Morris-Pratt-Algorithmus?uselang=de) unter der [Lizenz CC-BY-SA (3.0)](https://creativecommons.org/licenses/by-sa/3.0/legalcode).

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

Schauen wir uns den Algorithmus einmal Schritt für Schritt an folgendem Beispiel an:

Dies ist eine Folge von aufsteigend sortierten numerischen Werten. Wir möchten nun herausfinden, ob sich der numerische Wert **5** in dieser Folge befindet.

<lia-keep>
  <table style="border-style:hidden">
    <tr>
      <th>2</th>
      <th>3</th>
      <th>5</th>
      <th>8</th>
      <th>9</th>
      <th>14</th>
      <th>17</th>
      <th>21</th>
      <th>22</th>
    </tr>
  </table>
</lia-keep>

Um das herauszufinden vergleichen wir **jedes** Element mit dem gesuchtem Wert.
Dabei starten wir mit dem Element ganz links und gehen dann die Folge nach und nach durch, bis wir am Ende (rechts) angekommen sind.
Finden wir beim durchlaufen der Folge das gesuchte Element, dann kann die Suche abgebrochen und der **aktuelle** Index zurückgegeben werden.
Beachten Sie hierbei, dass der Index bei $0$ beginnt und nicht bei $1$.
Kommen wir allerdings am Ende der Folge an **ohne** das Element gefunden zu haben, dann wissen wir, dass sich der gesuchte Wert nicht in der Folge befindet.
Tritt dieser Fall ein, wird der Index $-1$ zurückgegeben.

*In diesem Beispiel ist jeweils das aktuell betrachtete Element Türkis markiert.*
*Falls dieses Element das gesuchte Element ist wird dies mit Grün gekennzeichnet, andernfalls mit Rot.*

Beginnen wir also mit dem ersten Element ganz links.
Dieses Element hat den Wert $2$.
Da $2 \neq 5$ gilt, ist dies nicht das Element das wir suchen.
Schauen wir uns also das nächste Element der Folge an.

<lia-keep>
  <table style="border-style:hidden;border-radius: 10px;">
    <tr>
      <th style="background-color:#54B6B5;">2</th>
      <th>3</th>
      <th>5</th>
      <th>8</th>
      <th>9</th>
      <th>14</th>
      <th>17</th>
      <th>21</th>
      <th>22</th>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius: 10px;">
    <tr>
      <th style="background-color:#F25B68;">2</th>
      <th>3</th>
      <th>5</th>
      <th>8</th>
      <th>9</th>
      <th>14</th>
      <th>17</th>
      <th>21</th>
      <th>22</th>
    </tr>
  </table>
</lia-keep>

Das nächste Element hat den Wert $3$.
Der Wert $3$ ist allerdings leider auch nicht der gesuchte Wert.
Somit können wir direkt weiter zum nächsten Element gehen.

<lia-keep>
  <table style="border-style:hidden">
    <tr>
      <th>2</th>
      <th style="background-color:#54B6B5;">3</th>
      <th>5</th>
      <th>8</th>
      <th>9</th>
      <th>14</th>
      <th>17</th>
      <th>21</th>
      <th>22</th>
    </tr>
  </table>
  <table style="border-style:hidden">
    <tr>
      <th>2</th>
      <th style="background-color:#F25B68;">3</th>
      <th>5</th>
      <th>8</th>
      <th>9</th>
      <th>14</th>
      <th>17</th>
      <th>21</th>
      <th>22</th>
    </tr>
  </table>
</lia-keep>

Jetzt sind wir beim dritten Element angekommen. Dieses hat einen Wert von $5$ und da $5 = 5$ gilt, ist dies unser gesuchtes Element.

<lia-keep>
  <table style="border-style:hidden">
    <tr>
      <th>2</th>
      <th>3</th>
      <th style="background-color:#54B6B5;">5</th>
      <th>8</th>
      <th>9</th>
      <th>14</th>
      <th>17</th>
      <th>21</th>
      <th>22</th>
    </tr>
  </table>
  <table style="border-style:hidden">
    <tr>
      <th>2</th>
      <th>3</th>
      <th style="background-color:#A6D492;">5</th>
      <th>8</th>
      <th>9</th>
      <th>14</th>
      <th>17</th>
      <th>21</th>
      <th>22</th>
    </tr>
  </table>
</lia-keep>

Der Sonderfall, dass der gesuchte Wert öfters vorkommt ist hier nicht relevant, da wir nur wissen möchten, ob sich Wert in der Folge befindet und nicht wie oft.
Somit kann die Suche jetzt abgebrochen und als Index $2$ zurückgegeben werden.

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
  return -1
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

Schauen wir uns den binäre Suche einmal Schritt für Schritt an folgendem Beispiel an:

Dies ist eine Folge von aufsteigend sortierten numerischen Werten. Wir möchten nun herausfinden, ob sich der numerische Wert $8$ in dieser Folge befindet.

<lia-keep>
  <table style="border-style:hidden">
    <tr>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>4</th>
      <th>5</th>
      <th>8</th>
      <th>9</th>
      <th>12</th>
      <th>13</th>
      <th>15</th>
    </tr>
  </table>
</lia-keep>

Um das herauszufinden vergleichen wir dieses mal **nicht** einfach jedes Element mit dem gesuchten Wert, sondern wir **halbieren** den Suchraum nach und nach so, dass möglichst wenige Vergleiche gemacht werden müssen, um eine Antwort zu erhalten. Dafür wählen wir jeweils den **mittleren** Eintrag im Suchraum aus und prüfen, ob dieser größer oder kleiner ist als der gesuchte Wert. Je nachdem, wie der Vergleich ausfällt, wird in der entsprechenden Hälfte des Suchraums weiter gesucht.

*In diesem Beispiel ist jeweils das aktuell betrachtete Element bzw. die Mitte des Suchraums mit Orange gekennzeichnet.*
*Die untere und obere Grenze des Suchraums ist zusätzlich mit Türkis bzw. Rot markiert.*

Zu Beginn beinhaltet der Suchraum noch die gesamte Folge.
Somit ist das erste Element, das wir betrachten, das mittlere Element der gesamten Folge. In diesem Fall ist dass das Element mit dem numerischen Wert $5$.
Da $5 < 8$ gilt, ist dies nicht das Element das wir suchen.
Durch den Vergleich können wir allerdings zusätzlich noch ausschließen, dass das gesuchte Element vor bzw. links von der $5$ steht. Wir brauchen unsere Suche also nur noch im rechten Teil der Folge fortsetzen.

<lia-keep>
  <table style="border-style:hidden">
    <tr>
      <th style="background-color:#54B6B5;">0</th>
      <th>1</th>
      <th>2</th>
      <th>4</th>
      <th style="color:#F38C3E;">5</th>
      <th>8</th>
      <th>9</th>
      <th>12</th>
      <th>13</th>
      <th style="background-color:#F25B68;">15</th>
    </tr>
  </table>
</lia-keep>

Der Suchraum beinhaltet nun nur noch den Teil der Folge, welcher sich rechts vom vorherigen Element, dem Element mit dem Wert $5$, befindet.
Die Mitte dieses neuen, verkleinerten Suchraums ist das Element mit dem Wert $12$.
Leider ist das aber auch noch nicht das gesuchte Element.
Diesmal gilt $12 > 8$, wodurch der Suchraum erneut verkleinert werden kann, und zwar auf den Bereich links von der aktuellen Mitte.

<lia-keep>
  <table style="border-style:hidden">
    <tr>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>4</th>
      <th>5</th>
      <th style="background-color:#54B6B5;">8</th>
      <th>9</th>
      <th style="color:#F38C3E;">12</th>
      <th>13</th>
      <th style="background-color:#F25B68;">15</th>
    </tr>
  </table>
</lia-keep>

Nun betrachten wir einen Suchraum der nur noch aus zwei Elementen besteht.
Das nach Definition mittlere Element ist in diesem Sonderfall das linke Element, d.h., das Element mit dem Wert $8$.
$8$ ist auch der Wert den wir Suchen.
Unsere Suche ist hiermit erfolgreich abgeschlossen.

<lia-keep>
  <table style="border-style:hidden">
    <tr>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>4</th>
      <th>5</th>
      <th style="background-color:#54B6B5;color:#F38C3E;">8</th>
      <th style="background-color:#F25B68;">9</th>
      <th>12</th>
      <th>13</th>
      <th>15</th>
    </tr>
  </table>
</lia-keep>

In diesem Beispiel benötigte die binäre Suche insgesamt drei Vergleiche um das Element zu finden. Zum Vergleich, die sequentielle Suche hätte bei dem selben Beispiel sechs Vergleiche benötigt. Es lässt sich sagen, dass in der Regel die binäre Suche effizienter als die sequentielle Suche ist. Es gibt allerdings Ausnahmen in denen dies nicht der Fall ist.

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
  return -1

def binarySearchRecursiv(input, low, up):
  # your code goes here ...
  return -1
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
> <!-- data-title="Fibonacci-Reihe" data-type="None" data-transpose="false" style="border-radius:0px;background-color:white;" -->
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

Schauen wir uns die fibonacci Suche einmal Schritt für Schritt an folgendem Beispiel an:

Dies ist eine Folge von aufsteigend sortierten numerischen Werten. Wir möchten nun herausfinden, ob sich der numerische Wert $133$ in dieser Folge befindet.

<lia-keep>
  <table style="border-style:hidden">
    <tr>
      <th>9</th>
      <th>19</th>
      <th>21</th>
      <th>34</th>
      <th>87</th>
      <th>102</th>
      <th>158</th>
      <th>159</th>
      <th>199</th>
      <th>205</th>
    </tr>
  </table>
</lia-keep>

Bei der binären Suche haben wir stets das mittlere Element des aktuellen Suchraums betrachtet, um diesen anschließend mithilfe dieses Elements zu halbieren. Bei der fibonacci Suche wählen wir auf eine andere Weise ein Element aus, damit der Suchraum noch effektiver geteilt werden kann. Und zwar wird jeweils das Element an der Indexposition betrachtet, welche die größt mögliche Fibonaccizahl im aktuellen Suchraum und zusätzlich nicht das letzte Element der Folge ist. **Als Beispiel:** Wenn der Suchraum eine Größe von $8$ hat, also Indexpositionen von $0$ bis $7$ besitzt, dann würde das Element an Indexposition $5$ zuerst betrachtet werden. Dies liegt daran, dass $F_5 = 5$ gilt und die nächst größere Fibonaccizahl $F_6 = 8$ schon außerhalb des Suchraums liegt.

*In diesem Beispiel ist jeweils das aktuell betrachtete Element bzw. die größte Fibonaccizahl (Indexposition) innerhalb des Suchraums mit Orange gekennzeichnet. Die untere und obere Grenze des Suchraums ist zusätzlich mit Türkis bzw. Rot markiert.*

Zu Beginn beinhaltet der Suchraum noch die gesamte Folge.
Die Folge hat eine größe von $10$, somit ist das erste Element, dass wir betrachten, das Element an Indexposition $8$. Denn $F_6 = 8$, $F_7 = 13$ und $8 < 9 < 13$.
Das Element an Indexposition $8$ hat einen Wert von $199$.
$199$ ist nicht die gesuchte Zahl $133$, ist aber größer als diese, wodurch wir den Suchraum auf den vorderen bzw. linken Teil der Folge einschränken können.

<lia-keep>
  <table style="border-style:hidden">
    <tr>
      <th style="background-color:#54B6B5;">9</th>
      <th>19</th>
      <th>21</th>
      <th>34</th>
      <th>87</th>
      <th>102</th>
      <th>158</th>
      <th>159</th>
      <th style="color:#F38C3E;">199</th>
      <th style="background-color:#F25B68;">205</th>
    </tr>
  </table>
</lia-keep>

Jetzt hat der Suchraum nur noch eine größe von $8$. In diesem Bereich ist die größt mögliche Fibonaccizahl $F_5 = 5$. Die nächst größere Fibonaccizahl $F_6 = 8$ ist nämlich schon außerhalb des aktuellen Suchraums. *Zur Erinnerung: Ein Suchraum der größe $8$ hat Indexpositionen von $0-7$.*
Das betrachtete Element ist also das mit dem numerischen Wert $102$. Der Wert $102$ ist kleiner als unsere gesuchte Zahl. Wir brauchen deshalb nur noch im rechten Teil des aktuellen Suchraums weiter suchen.

<lia-keep>
  <table style="border-style:hidden">
    <tr>
      <th style="background-color:#54B6B5;">9</th>
      <th>19</th>
      <th>21</th>
      <th>34</th>
      <th>87</th>
      <th style="color:#F38C3E;">102</th>
      <th>158</th>
      <th style="background-color:#F25B68;">159</th>
      <th>199</th>
      <th>205</th>
    </tr>
  </table>
</lia-keep>

Der Suchraum besteht jetzt nur noch aus zwei Elementen. Die größte Fibonaccizahl in diesem Bereich ist $F_0 = 0$. $F_1 = 1$ liegt nämlich schon nicht mehr im zugelassenem Bereich ($1 \lneqq 1$). Wir bertrachten somit das Element an Indexposition $0$ im aktuellen Suchraum, welches einen Wert von $158$ hat. Es gilt $158 > 133$, d.h. das gesuchte Element müsste links von dem aktuell betrachteten Element liegen. Allerdings gibt es im aktuellen Suchraum kein Element mehr, dass links vom aktuell betrachteten Element liegt. Unsere Suche ist dadurch erfolglos und kann beendet werden. Der Wert $133$ befindet sich nicht in der Folge.

<lia-keep>
  <table style="border-style:hidden">
    <tr>
      <th>9</th>
      <th>19</th>
      <th>21</th>
      <th>34</th>
      <th>87</th>
      <th>102</th>
      <th style="background-color:#54B6B5;color:#F38C3E;">158</th>
      <th style="background-color:#F25B68;">159</th>
      <th>199</th>
      <th>205</th>
    </tr>
  </table>
</lia-keep>


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
```python -Fibonacci-Folge
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
  return -1

def fibonacciSearchRecursiv(input, low, up):
  # your code goes here ...
  return -1
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

In dieser Selbstlerneinheit werden wir uns nur den naiven Algorithmus zur Textsuche anschauen. Falls interesse für weitere Algorithmen zur Suche in Texten besteht, können Sie sich den [Knuth-Morris-Pratt-Algorithmus](https://de.wikiversity.org/wiki/Kurs:Algorithmen_und_Datenstrukturen/Vorlesung/Textsuche_Knuth_Morris_Path) auf Wikiversity anschauen.


### Naiver Algorithmus zur Textsuche

In diesem Abschnitt betrachten wir den naiven Algorithmus zur Textsuche. Und zwar die direkte Lösung, **brute force**.

#### Beispiel

Schauen wir uns die **brute force** Methode einmal Schritt für Schritt an folgendem Beispiel an:

Dies sei ein Text, oder auch eine Folge von *character* (Zeichen), in dem wir das Wort **"abacab"** finden möchten.

<lia-keep>
  <table style="border-style:hidden;border-radius:0px;">
    <tr>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
</lia-keep>

Dafür gehen wir Zeichen für Zeichen, von vorne nach hinten, den Text durch und vergleichen, ob sich das gesuchte Wort an der jeweiligen Position befindet. Genauer gesagt kontrollieren wir, ob das gesuchte Wort an der jeweiligen Position **liegen könnte**, in dem wir das erste Zeichen des gesuchten Wortes mit dem Zeichen aus dem Text vergleichen. Stimmen sie überein, dann könnte das gesuchte Wort dort anfangen. Um das zu kontrollieren werden die jeweils nächsten Zeichen miteinander verglichen. Stimmen diese wieder überein wird mit den nächsten Zeichen fortgesetzt, und das so lange, bis das letzte Zeichen des gesuchten Wortes verglichen wurde oder bis die zu vergleichenden Zeichen nicht mehr miteinander übereinstimmen. Tritt ersteres ein, dann wurde das Wort erfolgreich gefunden. Tritt aber letzteres ein, dann kann das gesuchte Wort an dieser Position im Text **nicht** mehr liegen. Die Suche würde dann an der nächsten Position im Text von vorne begonnen werden. Hierbei muss beachtet werden, dass keine Position als möglicher Anfang des gesuchten Wortes ausgelassen wird.

*In diesem Beispiel ist jeweils das aktuell betrachtete Zeichen, welches mit dem Text verglichen wird, Orange gekennzeichnet. Der Bereich in dem das Wort jeweils vermutet wird ist mit Türkis markiert. Ist ein einzelner Vergleich erfolgreich, ist dies mit Grün gekennzeichnet, anderenfalls mit Rot.*

Wir starten mit dem ersten Zeichen aus dem Text. Dieses ist das Zeichen **a**. Unser gesuchtes Wort ("abacab") fängt ebenfalls mit dem Zeichen **a** an. Der erste Vergleich ist also erfolgreich und wir können die beiden nächsten Zeichen miteinander vergleichen. Diese stimmen ebenfalls überein (**b = b**), weshalb wir mit den nächsten weitermachen können, welche wiederum übereinstimmen, usw. Beim letzten Zeichen des gesuchten Wortes schlägt unser Vergleich mit dem Zeichen aus dem Text allerdings fehl (**a $\neq$ b**). Das Wort liegt also nicht an dieser Position (Index 0).

<lia-keep>
  <table style="border-style:hidden;border-radius:0px;">
    <tr style="border-radius:10px;">
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">b</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">b</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">b</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">b</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">b</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">b</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">c</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">b</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">c</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#F25B68;border-radius:0px;">a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
</lia-keep>

Die Suche geht nun beim nächsten Zeichen im Text von vorne los, d.h. mit dem Zeichen auf der Indexposition 1. Dieses Zeichen ist ein **b**. Wir benötigen aber ein **a** am Anfang des Wortes. Das gesuchte Wort kann hier also auch nicht liegen.

<lia-keep>
  <table style="border-style:hidden;border-radius:0px;">
    <tr style="border-radius:10px;">
      <td>a</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">b</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr style="border-radius:10px;">
      <td>a</td>
      <td style="background-color:#F25B68;border-radius:0px;">b</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
</lia-keep>

Führen wir die Suche an der nächsten Position fort ist der erste Vergleich wieder erfolgreich. Leider schlägt der Vergleich mit dem zweiten Zeichen fehl. An dieser Stelle liegt das gesuchte Wort somit auch nicht.

<lia-keep>
  <table style="border-style:hidden;border-radius:0px;">
    <tr style="border-radius:10px;">
      <td>a</td>
      <td>b</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">b</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr style="border-radius:10px;">
      <td>a</td>
      <td>b</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">b</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr style="border-radius:10px;">
      <td>a</td>
      <td>b</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#F25B68;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">b</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td>c</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
</lia-keep>

---

**. . .**

---

Führen wir dieses Prozedere fort, so bleiben wir zunächst noch einige male erfolglos. Und zwar so lange, bis wir an Indexposition 10 angekommen sind. Kontrollieren wir Zeichen für Zeichen, ob unser gesuchtes Wort dort liegt und anfängt, dann stellen wir fest, dass **alle** Vergleiche erfolgreich sind. Wir haben unser Wort gefunden. Es fängt an der Indexposition 10 an und hört an der Indexposition 15 auf.

<lia-keep>
<table style="border-style:hidden;border-radius:0px;">
  <tr style="border-radius:10px;">
    <td>a</td>
    <td>b</td>
    <td>a</td>
    <td>c</td>
    <td>a</td>
    <td>a</td>
    <td>b</td>
    <td>a</td>
    <td>c</td>
    <td>c</td>
    <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">a</td>
    <td style="background-color:#54B6B5;border-radius:0px;">b</td>
    <td style="background-color:#54B6B5;border-radius:0px;">a</td>
    <td style="background-color:#54B6B5;border-radius:0px;">c</td>
    <td style="background-color:#54B6B5;border-radius:0px;">a</td>
    <td style="background-color:#54B6B5;border-radius:0px;">b</td>
    <td>a</td>
    <td>a</td>
  </tr>
</table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr style="border-radius:10px;">
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">b</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr style="border-radius:10px;">
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">b</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr style="border-radius:10px;">
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">b</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr style="border-radius:10px;">
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">b</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">c</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;border-radius:0px;">b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr style="border-radius:10px;">
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">b</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">c</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#54B6B5;color:#F38C3E;border-radius:0px;">b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
  <table style="border-style:hidden;border-radius:0px;">
    <tr style="border-radius:10px;">
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>a</td>
      <td>a</td>
      <td>b</td>
      <td>a</td>
      <td>c</td>
      <td>c</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">b</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">c</td>
      <td style="background-color:#A6D492;border-radius:0px;">a</td>
      <td style="background-color:#A6D492;border-radius:0px;">b</td>
      <td>a</td>
      <td>a</td>
    </tr>
  </table>
</lia-keep>

Die Suche kann nun beendet werden. Der Rest des Textes muss nichtmehr kontrolliert werden, da wir nur wissen wollten, ob das gesuchte Wort vorkommt und nicht, ob es mehrmals drinnen vorkommt.

#### Implementierung

In diesem Kapitel werden Sie den Algorithmus selber schrittweise in Python implementieren. Der Code-Rahmen und Möglichkeiten Ihren Code zu testen sind jeweils schon gegeben, Sie müssen nur an den mit *"your code goes here ..."* gekennzeichneten Stellen ihren Code einfügen. Bei Bedarf ist es auch möglich eigene Testfälle zu schreiben.

Falls Sie Hilfe beim Einstieg in Python brauchen, finden Sie diese z.B. [hier](https://learnxinyminutes.com/docs/de-de/python-de/), falls es ganz schnell gehen muss oder [hier](https://www.python-lernen.de/), falls es etwas ausführlicher sein soll.

<!--  style="background-color:#A6D492;" -->
> ## Ziel dieses Kapitels
>
> Nach diesem Kapitel sollten Sie in der Lage sein, den naiven Alogorithmus zur Textsuche selbstständig zu implementieren.

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
TEXT = "abababcbababcababcab"

def bruteForceSearch(pattern):
  # your code goes here ...

  return -1
```
<!-- data-readOnly="True"  style="display:block"-->
``` python -main.py
from bruteForceSearch import bruteForceSearch, TEXT

if __name__ == "__main__":
    print "Bitte geben Sie eine beliebige zu suchende Zeichenfolge ein:"
    input = raw_input()
    index = bruteForceSearch(input)
    if index != -1:
      print "Die Zeichenfolge {} befindet sich in dem Text \n{} \nan dem Index {}.".format(input, TEXT, index)
    else:
      print "Die Zeichenfolge {} befindet sich nicht in dem Text \n{}.".format(input, TEXT)
```
@LIA.eval(`["bruteForceSearch.py", "main.py"]`, `python -m compileall .`, `python main.pyc`)

<details class="panel">
<summary class="button">**Schritt 1:**</summary>

<p class="panel-content">
Damit der Text nach einem bestimmten Teilwort (**Pattern**) durchsucht werden kann, muss der Text und das gesuchte Teilwort als **Liste** zur Verfügung stehen. Ein Listenelement ist dabei jeweils ein einzelnes Zeichen.

Speichern Sie also zunächst den gegebenen Textkorpus und das übergebene Pattern so ab, sodass die Bedingung erfüllt ist.
</p>
</details>

<details class="panel">
<summary class="button">**Schritt 2:**</summary>

<p class="panel-content">
Schreiben Sie als nächstes einen Codeabschnitt, in dem der Textkorpus von vorne nach hinten durchlaufen wird. Zusätzlich soll dabei bei **jedem** Element des Textes kontrolliert werden, ob das erste Zeichen des gesuchten Patterns mit dem aktuellen Element übereinstimmt.

_**Hinweis:** Damit der naive Alogorithmus funktioniert muss nicht der gesamte Textkorpus durchlaufen werden._
</p>
</details>

<details class="panel">
<summary class="button">**Schritt 3:**</summary>

<p class="panel-content">
Erweitern Sie Ihren Codeabschnitt nun wie folgt:

- Tritt der Fall ein, dass die beiden zu vergleichenden Zeichen **übereinstimmen**, dann durchlaufen Sie das Pattern (und den Textkorpus) weiter und kontrollieren Sie, ob die folgenden Zeichen ebenfalls übereinstimmen.
- Tritt der Fall ein, dass die beiden zu vergleichenden Zeichen **nicht** übereinstimmt, dann brechen Sie den aktuellen Durchlauf des Patterns ab und starten Sie die Suche beim nächsten Element des Textes von vorne.

_**Hinweis:** Verwenden Sie zwei Schleifen. Eine zum durchlaufen des Textes und eine zum durchlaufen des Patterns._

_**Hinweis:** Achten Sie im zweiten Fall darauf, dass die Suche beim richtigen Zeichen des Textes fortgesetzt wird. Am Ende sollte jedes Zeichen des Textes einmal mit dem ersten Zeichen des Patterns verglichen worden sein (Falls das Pattern nicht vorher schon gefunden wurde)._
</p>
</details>
<details class="panel">
<summary class="button">**Schritt 4:**</summary>

<p class="panel-content">
Jetzt soll noch der Fall hinzugefügt werden, dass das Pattern **vollständig** gefunden wurde. Tritt dies ein, soll der Index zurückgegeben werden an dem sich das erste Zeichen des Patterns im Textkorpus befindet.

_**Hinweis:** Das Pattern wurde vollständig im Textkorpus gefunden, wenn das letzte Zeichen des Pattern erfolgreich mit dem "aktuellen" Zeichen des Textes verglichen wurde._
</p>
</details>
