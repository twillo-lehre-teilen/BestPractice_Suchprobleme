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

link:

import:
-->

# Suchprobleme

> # Lizenzhinweis
>
> Der Kurs *Suchprobleme*, von Lennart Rosseburg für twillo, ist lizenziert unter der [Lizenz CC-BY-SA (3.0)](https://creativecommons.org/licenses/by-sa/3.0/legalcode).
>
> #### Unter Nutzung von
>
> - Dem [Kapitel "Suchen"](https://de.wikiversity.org/wiki/Kurs:Algorithmen_und_Datenstrukturen/Vorlesung/Suchen) aus dem [Kurs "Algorithmen und Datenstrukturen"](https://de.wikiversity.org/wiki/Kurs:Algorithmen_und_Datenstrukturen), von Wikiversity unter der Beteiligung folgender [Autor:innen](https://de.wikiversity.org/w/index.php?title=Kurs:Algorithmen_und_Datenstrukturen/Vorlesung/Sortieren_Grundlagen&action=history), unter der [Lizenz CC-BY-SA (3.0)](https://creativecommons.org/licenses/by-sa/3.0/legalcode).

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
> Nach diesem Kapitel sollten Sie die Vorgehensweisen von vorgestellten Suchalgorithmen verstanden haben, sowie die einzelnen Schritte benennen und anwenden können. Außerdem sollten Sie in der Lage sein die Algorithmen schrittweise zu implementieren.

---

<h4>Grundlagen - lineare Datenstrukturen</h4>
<h5>Defintion</h5>

Eine lineare Datenstruktur $L$ ist eine Sequenz $L=(a_{1}...,a_{n})$. Die lineare Datenstruktur ordnet Elemente (entweder primitive Datentypen oder komplexere Datenstrukturen) in einer linearen Anordnung an.

> ## Beispiel
>
> Zahlenfolgen: $[5,4,6,1,3,2]$
>
> Strings: $[L,I,N,E,A,R]$

<h5>Atomare Operationen</h5>

Zu den Operationen gehören Lesen mit

- $get(i)$: Element an Position $i$ lesen
- $first()$: erstes Element lesen
- $last()$: letztes Element lesen
- $next(e)$: Element nach Element $e$ lesen

und Schreiben mit

- $set(i,e)$: Element an Position $i$ auf $e$ setzen
- $add(i,e)$: Element $e$ an Position $i$ einfügen
- $del(i)$: Element an Position $i$ löschen

<h5>Arrays und Listen</h5>

Es gibt zwei Möglichkeiten lineare Datenstrukturen zu realisieren. Entweder Arrays oder (verlinkte) Listen. Arrays belegen einen zusammenhängenden Bereich im Speicher. Elemente einer verlinkten Liste können beliebig verteilt sein. Ob zur Realisierung einer linearen Datenstruktur ein Array oder eine Liste verwendet wird, hängt von der Anwendung ab. Arrays werden meist für statische Datenstrukturen verwendet, d.h. wenn die Länge des Arrays nicht verändert wird. Listen werden meist für dynamische Datenstrukturen verwendet, d.h. wenn die Länge variabel ist. Zu den positiven Eigenschaften von Arrays zählen der schneller Zugriff auf Einzelelemente durch den Index. Zu den negativen Eigenschaften von Arrays zählen das sehr aufwändige Einfügen der Elemente. Zu den positiven Eigenschaften von Listen zählen die relativ effiziente Manipulation, zu den negativen Eigenschaften der ineffiziente Direktzugriff.

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
