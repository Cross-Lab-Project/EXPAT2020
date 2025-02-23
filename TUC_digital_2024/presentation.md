<!--
author:   Sebastian Zug; André Dietrich

email:    sebastian.zug@informatik.tu-freiberg.de

version:  0.1.1

language: de

narrator: Deutsch Female

icon:     https://media.aubi-plus.com/institution/thumbnail/3f3de48-technische-universitaet-bergakademie-freiberg-logo.jpg

link:     style.css

import:   https://raw.githubusercontent.com/liaTemplates/TextAnalysis/main/README.md
          https://raw.githubusercontent.com/LiaScript/CodeRunner/master/README.md
          https://raw.githubusercontent.com/LiaTemplates/LiveEdit-Embeddings/refs/tags/0.0.1/README.md

@runManimAnimation
```text   -manim.cfg
[CLI]
background_color = BLACK
media_dir = .
video_dir = .
images_dir = .
#verbosity = ERROR
#quiet = True
#progress_bar = none 
```
@LIA.eval(`["main.py","manim.cfg"]`, `none`, `manim render --format=webm main.py MyScene -o animation`)
@end

-->

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/Cross-Lab-Project/presentations/refs/heads/main/TUC_digital_2024/presentation.md)

# Warum braucht OER eine gemeinsame Sprache?!

<h2>Visionen für die kollaborative Gestaltung von Offenen Lehr-Lern-Materialien</h2>

---------------------

> Umfrage: In welchen Format haben Sie bereits OER öffentlich zur Verfügung gestellt?

- [(pdf)] pdf
- [(office)] Office Format (word, powerpoint, ...)
- [(opal)] Opal
- [(liascript)] LiaScript
- [(others)] andere Formate

----------------------

<h5>
<p>Prof. Dr. Sebastian Zug, Dr. Andre Dietrich</p>
<p>TU Bergalakdemie Freiberg</p>
<p>November 2024</p>
</h5>

<div>

---

</div>

## Motivation

                  {{0-1}}
******************************************

> Wer von Ihnen kennt die Videos von `3brown1blue`? 

!?[3blue1brown](https://www.youtube.com/watch?v=r6sGWTCMz2k&t=719s "Video aus der Reihe zu Differentialgleichungen mit 17 Millionen Views")

******************************************

                  {{1-2}}
******************************************

> Was macht dieses Videos so besonders?

- __gut designte Animationen__
- eigene Lerngeschwindigkeit (Stop, Pause, Rückspulen)
- weiterführende Materialien als Links

> Was sind Beschränkungen?

| Limitierung                                         | Erläuterungen                                                                                                                                               |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _Fehlende Anpassungsfähigkeit_                      | Das finale Material - ein Video - ist statisch und kann nur mit erheblichen Aufwand auf individuelle didaktische Ziele oder Vorkenntnisse angepasst werden. |
| _Einstiegshürde Vorwissen/Fähigkeiten_              | Die Umsetzung der Animationen, Gleichungen usw. erfordert spezielle Kenntnisse / Technikaffinität.                                                          |
| _Interaktivität ist konzeptionell nicht vorgesehen_ | Ein Eingriff des Nutzenden ist nicht vorgesehen.                                                                                                            |
| _Abhängigkeit von einer Plattform_                  | Die Videos sind nur bei bestehender Internetverbindung verfügbar.                                                                                           |

******************************************

## Lösungsansatz

> __Wir trennen Darstellung und Inhalt! Alle Elemente werden soweit wie möglich durch eine rein textuelle Repräsentation ausgedrückt.__

### Konzept

Was ist damit gemeint?

```markdown @embed.style(height: 550px; min-width: 100%; border: 1px black solid)
# Vom Text zur Darstellung

__Mathematik__

$f(x) = x^2$

__Tabellen__

| X | B(y) | C(y) |
|---|:----:|:----:|
| 1 |   2  |   3  |
| 4 |   5  |   6  |

__Sprache__

> Click to run!
>
> {{|> Deutsch Female}}
> Markdown ist eine vereinfachte Auszeichnungssprache, die der Ausgangspunkt unserer Entwicklung von LiaScript war.
```

### Konsequenzen und Chancen

<details>

<summary>__Fehlende Anpassungsfähigkeit__ -> Generelle Editierbarkeit </summary>

Die textuellen Repräsenation eröffnet die Möglichkeit, dass
+ jeder Nutzende Materialien anpassen kann und
+ eine Versionierung der Materialien mit etablierten Tools realisiert werden.

!?[Einbettung Studierender bei der Bearbeitung von Materialien](https://github.com/TUBAF-IfI-LiaScript/.github/assets/10922356/00a24602-dc63-4b9a-894b-80967b914513)

</details>

<details>

<summary>__Einstiegshürde Vorwissen/Fähigkeiten__ -> Generierung von Inhalten </summary>

Die textuelle Repräsentation erlaubt den extensiven Einsatz von KIs für die Textgenerierung.

``` text
Generiere mir eine Animation, die die Multiplikation von
zwei Matrizen mit manim im Stil von 3blue1brown zeigt.
```

```python -manim.py
from manim import *

class MyScene(Scene):
    def construct(self):
        # Define the matrices
        matrix_A = MathTex(r"\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}", color=BLUE)
        matrix_B = MathTex(r"\begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix}", color=GREEN)
        matrix_C = MathTex(r"\begin{bmatrix} 19 & 22 \\ 43 & 50 \end{bmatrix}", color=YELLOW)

        # Position the matrices on the screen
        matrix_A.move_to(LEFT * 3)
        matrix_B.move_to(ORIGIN)
        equals_sign = MathTex("=").next_to(matrix_B, RIGHT)
        matrix_C.next_to(equals_sign, RIGHT)

        # Display matrices and equals sign
        self.play(Write(matrix_A), Write(matrix_B))
        self.play(Write(equals_sign), Write(matrix_C))

        # Highlight the first row and first column
        row_rect = SurroundingRectangle(matrix_A[0][2:4], color=BLUE, buff=0.1)
        col_rect = SurroundingRectangle(matrix_B[0][0:2], color=GREEN, buff=0.1)
        self.play(Create(row_rect), Create(col_rect))

        # Compute the first element of the result (19)
        dot_prod_1 = MathTex("1 \\cdot 5 + 2 \\cdot 7 = 19")
        dot_prod_1.next_to(matrix_A, UP)
        self.play(Write(dot_prod_1))
        self.play(Transform(dot_prod_1, matrix_C[0][2:4].copy()))
        self.play(FadeOut(dot_prod_1))

        # Final pause to view the result
        self.wait(2)
```
@runManimAnimation

</details>

<details>

<summary> __Fehlende Interaktivität__ -> Sprachfeatures und Plugins </summary>

Die Sprachkonzepte von LiaScript und die Einbettung von Plugins ermöglichen die Integration von interaktiven Elementen.

```markdown @embed.style(height: 550px; min-width: 100%; border: 1px black solid)
# Quiz

Wann wurde die TU Chemnitz gegründet?

- [( )] 1886
- [(X)] 1986
- [( )] 1996
```


````markdown @embed.style(height: 550px; min-width: 100%; border: 1px black solid)

<!--
import: https://github.com/liascript/CodeRunner
-->

# Programmierübungen

Debuggen Sie den nachfolgenden Code

```cpp                     ErroneousHelloWorld.cpp
#include <iostream>

imt main() {
	std::cout << "Hello World!'';
	std::cout << "Wo liegt der Fehler?";
	return 0;
}
```
\@LIA.evalWithDebug(`["main.cpp"]`, `g++ main.cpp -o a.out`, `./a.out`)
````

</details>

<details>

<summary>__Abhängigkeit von einer Plattform__ -> Exportierbarkeit </summary>

Durch die Trennung von Inhalt und Ausführung können die Kurse in LMS, Progressive Web Apps oder als PDF exportiert werden.

!?[LiaScript auf Nokia-Basis](https://www.youtube.com/watch?v=U_UW69w0uHE)

</details>

## Warum funktioniert das bisher nicht?

             {{0-1}}
******************************************

> Wir nutzen die falschen Werkzeuge!

<!-- data-type="none"-->
| OPAL OER Materialien | Anzahl | Limitierung                              |
| -------------------- | -----: | ---------------------------------------- |
| Kurse                |   3828 | + nur über "Umwege" exportierbar         |
|                      |        | + Infrastruktur für Ausführung notwendig |
| einzelne Dateien     |  11322 | + überwiegend unveränderliche Inhalte    |
|                      |        | + fehlende Metadaten                     |

<!-- data-type="barchart" data-show="true" data-xlabel="Dateitypen"
     data-ylabel="Anteil in Prozent" data-title="Dominierend Dateitypen im OPAL OER Datenbestand" -->
| Format | Anteil [%]  |
| ------ | ---------: |
| pdf    |       53.0 |
| jpg    |       10.5 |
| mkv    |        8.8 |
| mp4    |        5.9 |
| png    |        5.0 |
| zip    |        4.4 | 
| html   |        3.9 |
| docx   |        3.8 |
| pptx   |        2.4 |
| xlsx   |        1.9 |

> Untersuchungen des Projektes "OER-Connected Lectures", dass durch den AK Elearning Sachsen 2024/25 gefördert wird (https://github.com/TUBAF-IFI-ConnectedLecturer)

******************************************

             {{1-2}}
******************************************

> Wir brauchen Überwindung auf Seiten der Lehrenden und der Studierenden!

![Screenshot Shrimpp](SceenshotShrimpp.png "Screenshot der [Shrimpp-Plattform](https://www.shrimpp.de/) (Uni Leipzig) zum kooperativen Editieren von pdf-Dateien")

******************************************

## Ausblick und Vision

1. Wir brauchen eine Loslösung von Frameworks und Plattformen und einen Fokus auf die Inhalte als Gelingensbedingung für OER.

2. AI wird die Diskussionen im Bereich von OER dahingehend verändern, dass die Generierung von Inhalten und die Anpassung an individuelle Bedürfnisse im Vordergrund stehen.

3. Die Abbildung der Inhalte als Text eröffnet in viel größerem Maße die Möglichkeit zu kollaborativer Erstellung und Anpassung.

--------------------

> ... Ach so, das Ganze ist keine "Spielwiese für Nerds", sondern schon in der TU Chemnitz fest verankert. https://www-user.tu-chemnitz.de/~herbs/lehrmaterial.html

--------------------------------

> Dieser Vortrag ist eine Open Educational Resource (OER) und steht unter der Lizenz [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.de).

+ Alle enthaltenen Inhalte können frei verwendet werden und sind unter https://github.com/Cross-Lab-Project/presentations/blob/main/TUC_digital_2024/presentation.md verfügbar