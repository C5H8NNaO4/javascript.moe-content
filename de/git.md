

## [Git](https://git-scm.com/)

### Was ist Git?

Git ist ein Versionskontrollsystem, mit dem du alle Änderungen an einem Codebestand / Repository nachverfolgen kannst. Es ermöglicht effektive Zusammenarbeit und erlaubt es mehreren Personen, gleichzeitig am gleichen Code zu arbeiten.

Es gibt ein paar Konzepte, die du kennen solltest: _Repositories_, _PRs_, _Branches_ und _Commits_.

Sobald du diese Konzepte besser verstanden hast, kannst du Git für deine eigenen Projekte verwenden.

In diesem Artikel gehe ich nicht auf die absoluten Grundlagen ein, da es bereits eine Vielzahl an Details dazu gibt. Es gibt einen [ausführlichen Guide zu Git für Einsteiger](https://stackoverflow.com/questions/315911/git-for-beginners-the-definitive-practical-guide) auf StackOverflow, falls du noch nie mit Git gearbeitet hast.

Dennoch gibt es ein paar wichtige Dinge, die du auch kennen solltest, wenn du bereits Erfahrung mit Git hast — besonders, wenn du mit mehreren Personen am gleichen Projekt oder Feature arbeitest.

Das Erste, das du lernen solltest, ist der effektive Einsatz von _Branches_. So kannst du die Arbeit in Features, Fixes und Releases aufteilen. Das macht es einfacher, separat an Features zu arbeiten und trotzdem stabile Release-Zyklen zu behalten, wenn dein Projekt komplexer wird.

### Branching-Modell

Normalerweise arbeitest du an _Feature_<sup>1</sup>- und _Hotfix_-Branches. Sobald die Entwicklung eines _Features_ den [_DoD_](https://www.scrum.org/resources/what-definition-done) erfüllt, kann es in den aktuellen _Release_<sup>2</sup>-Branch gemerged werden. Es ist sinnvoll, _Release_-Branches zu verwenden, die Features und Bugfixes sammeln, bis die Anforderungen für die nächste Version erfüllt sind<sup>3</sup>.

<sub>1 - Features stellen eine wertsteigernde Erweiterung des Produkts dar und sind in der Regel in sich abgeschlossen. Es ist sinnvoll, für jedes Feature einen eigenen Branch zu haben, da dieser alle Änderungen enthält, die für die Wertsteigerung notwendig sind.</sub>
<sub>2 - Bei der Produktentwicklung gibt es in der Regel Release-Zyklen. Ein Release ist eine Sammlung von Features, die eine abgeschlossene Version eines Produkts markieren.</sub>
<sub>3 - Das bedeutet, alle Features erfüllen ihren DoD, alle Tests laufen erfolgreich durch. QA und Design sind abgenommen. Linter und Compiler werfen keine Fehler und die Pipeline ist _grün_ (läuft erfolgreich durch).</sub>

Sobald ein Release-Branch bereit ist und in Produktion deployed wurde, sollte er abgeschlossen (_locked_) werden. Danach kann er in den _Master_-Branch gemerged und mit der entsprechenden Versionsnummer getaggt werden.

Hier ist ein Artikel von Atlassian über [verschiedene Branching-Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows), darunter auch [_GitFlow_](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).

Sobald du dich mit Branches, Features und Release-Zyklen auskennst, kannst du anfangen, mit anderen an einem Projekt zu arbeiten.

#### Ein paar Dinge, die man wissen sollte, wenn man zusammen an einem Feature-Branch arbeitet:

Dazu gehören z. B. das Rebasen auf den Upstream und das Erhalten der Historie beim Mergen.

### Was ist der Unterschied zwischen `git rebase` und `git merge`?

<stackoverflow  url="https://stackoverflow.com/a/16666418/1487756">
Siehe diese [Frage](https://stackoverflow.com/questions/16666089/whats-the-difference-between-git-merge-and-git-rebase) auf StackOverflow.
</stackoverflow>

### Wann sollte man `git rebase` verwenden und wann `git merge`?

Sowohl das Mergen als auch das Rebasen sind manchmal notwendig – und es ist recht einfach zu entscheiden, welches wann angebracht ist.

Es hängt vom Zustand deines **lokalen** Branches und der *Richtung* der Änderungen ab, die du holen oder pushen willst.

Wenn jemand anderes während deiner Arbeit Änderungen an demselben Branch vorgenommen hat, musst du den Remote-Branch **rebasen**, bevor du deine Commits in den Remote-Branch **mergest**.

Angenommen, du arbeitest mit mehreren Personen am gleichen Feature-Branch. Während du lokal daran arbeitest, hat jemand anderes seine Änderungen auf den Remote-Feature-Branch _gemerged_ / gepusht. Wenn du jetzt deine eigenen Änderungen _mergen_ möchtest, musst du zuerst die Upstream-Änderungen _pullen_.

Hier kommt das _Rebase_ ins Spiel. Es verschiebt deine lokalen Commits hinter bzw. auf die Änderungen, die andere am Remote-Branch vorgenommen haben, und erzeugt so eine lineare Commit-Historie, die sauber in den Remote-Branch gemerged werden kann.

An dieser Stelle stellt sich die nächste Frage: Wie mergt man einen _Feature_-Branch in einen _Release_-Branch?

Du hast dabei im Grunde zwei Optionen: `--no-ff` und `--squash`

### Was ist der Unterschied zwischen `--no-ff` und `--squash`?

Diese beiden Optionen beeinflussen, wie deine Commit-Historie aussieht – was wiederum Auswirkungen darauf hat, wie einfach es ist, Änderungen später rückgängig zu machen.

Grundsätzlich sollte Einfachheit bevorzugt werden, und eine _lineare_ Historie ist einfacher als eine mit vielen komplexen Merge-Commits.

Wenn du eine Serie von Commits (ein Feature) in deinen Release-Branch mergst, kannst du entscheiden, ob du jeden einzelnen Commit des Feature-Branches in der Historie behalten möchtest (`--no-ff`) oder ob du alle Commits zu einem einzigen Commit zusammenfassen möchtest (`--squash`).

Das Standardverhalten von Git ist ein _Fast Forward_, bei dem dein HEAD einfach auf die Spitze des Feature-Branches gesetzt wird. Dadurch geht die Information verloren, dass ein Feature in einem separaten Branch entwickelt wurde, aber alle Commits mit ihren individuellen Commit-Nachrichten bleiben erhalten.

Diese Commit-Nachrichten sind wichtig, weil sie z. B. in der IDE neben dem jeweiligen Code angezeigt werden – was bei der Zusammenarbeit sehr hilfreich ist.

`--squash` hingegen _quetscht_ alle Commits in einen einzigen Commit zusammen, was bedeutet, dass jede Codezeile im _Blame_ dieselbe Commit-Nachricht anzeigt.

Das ist in vielen Fällen in Ordnung – du solltest dir aber bewusst sein, dass du bei Bedarf nur den gesamten Squash-Commit zurückrollen kannst, nicht einzelne Teile daraus.

Der präziseste Befehl, der die Historie, Commit-Nachrichten für `blame` und die Möglichkeit, einzelne Commits zurückzunehmen, erhält, lautet:

```bash
git  merge  feat/my-feature  --no-ff
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNjgwMDQ1NTVdfQ==
-->