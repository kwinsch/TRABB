# Train Audio Backup Bus Specification
Version 0
Author: Kevin Bortis <kevin.bortis@ruf.ch>

## Abstract
Das folgende Dokument beschreibt ein analoges Audio Bussystem, das bei Ausfall des IP-Netzwerkes verwendet werden kann. Im Gegensatz zu dem in den 60er-Jahren entwickelten UIC568, werden andere Verfahren zur Signalisation verwendet, um das System flexibler und auf Hardwareseite günstiger zu gestalten. UIC568 erlaubt im Maximum 20 Teilnemer auf den Leitungen 5/6 und 7/8. Dies war keine Limitierung, solange keine Mehrfachtraktionen und Doppelstöckige Wagen eingesetzt wurden. PA-Verstärker gab es höchstens einen pro Wagen. Heute sind bis zu 16 Verstärker pro Zug und 32 in der Mehrfachtraktion vorhanden. Zudem kommen zusätzliche Audiogeräte wie Notsprechsysteme und Zugsfunk. Notsprechsysteme können an der Anzahl bis zu 200 Knoten pro Mehrfachtraktion ausmachen. Gemäss UIC568 müssen 20 mA bei 24 VDC pro Knoten und Funktion zur verfügung gestellt werden. Bei bei 264 Teilnemern, wären dies über 10 A nur für UIC 5/6 und 7/8! Somit ist UIC568 für moderne Züge nicht mehr geeignet.

Die TRAB-Bus kompatiblen Geräte verfügen über hochohmige Signalisationseingänge und sind auf der Audio Seite voll UIC Wire 3/4 kompatibel. So ist ein Mix Betrieb in begrenzt möglich. Der Bus erlaubt den Anschluss von min. 254 Geräten.

## Definitionen
### Begriffe
**TRABB**: Train Audio Backup Bus
**Endgerät**: An den TRABB angeschlossenes Gerät.
**DTMF**: Dual-tone multi-frequency
**High**: Digital 1, resp. > 10 VDC
**Low**: Digital 0, resp. 0 VDC
**PPS**: Pulses Per Second
**UIC568**: UIC Codex 568
**PA**: Public Announcement

### Impulswahlverfahren
Einige Funktionen des TRABB benutzten Impulswahlverfahren auf einer seperaten Signalisationsleitung.

#### Impulsdauer
Die Impulsdauer beträgt nominal 100 ms. Die komplette Impulsdauer darf eine maximale Tolleranz von 10% aufweisen.

Minimumimpulsdauer: 90 ms
Maximumimpulsdauer: 110 ms

Daraus resultiert ein Pulsintensität von 9-11 Impulse pro Minute (PPS).

#### Impuls-Pause-Verhältnis
Impuls-Pause-Verhältnis ist 60/40. Dabei ist das Signal nominal 60 ms Logic "High" und 40 ms Logic "Low". Für Tolleranzen siehe Impulsdauer.

#### Pausen
Zwischen zwei Ziffern ist eine Pause von nominal 200 ms einzusetzen. Durch eine Pause wird das Ende einer Ziffer signalisiert.

#### Wahlzahlen
Beim Impulswahlverfahren werden Zahlen von 0-9 nach folgendem Schema übertragen:

```
'1' : 1 Pulse
'2' : 2 Pulse
'3' : 3 Pulse
'4' : 4 Pulse
'5' : 5 Pulse
'6' : 6 Pulse
'7' : 7 Pulse
'8' : 8 Pulse
'9' : 9 Pulse
'0' : 10 Pulse
```

Wird im Text auf `'3'` verwiesen, so ist die Wahl der Ziffer 3 gemeint.

## Elektrisches Interface
Das Elektrische Interface des TRABB besteht aus insgesammt 4-Aderpaaren. Dabei müssen 2-Aderpaare als Bus durch den ganzen Zug geführt werden (Leitung AB und WL). Die weiteren 2-Aderpaare können zur Erweiterung des Systems innerhalb eines Zugswagens eingesetzt werden (Leitungen RS und PQ).

### 2-Draht Audio Leitung (TRABB-AB)
Zur Übertragung der Audio Signale wird eine verdrillte 2-Draht Leitung verwendet. Die aktiven Endgeräte sind dabei mit standard 600 Ohm Audio-Transformator an die Leitung angeschlossen. Durch die beidseitige Impedanzanpassung wird das Echo minimiert. Sind die Endgeräte nicht aktiv, so ist der Lasterzeugende Transformator Beidseitig von der Audioleitung zu trennen.

Zur Wahrung der Kompatibilität zu der UIC568 - Leitung 3/4, tolleriert der Bus eine übergelagerte DC Spannung von maximal 37 VDC. Diese Spannung wird bei verwendung des TRABB ignoriert.

An das TRABB-AB Leitungspaar, dürfen maximal 17 Endgeräte gleichzeitig aktiv angeschlossen werden (Verbundener 600 Ohm Audio-Transformator). Beispiel: 16 PA-Verstärker in den Zugsabteilungen, 1 Sender. Beim Anschluss einer höheren Anzahl Geräte sinkt die Impedanz so stark, dass eine Signalübertragung nicht mehr garantiert werden kann.

#### Elektrische Eigenschaften
Siehe UIC568 W 3/4

#### Leitungsnamen
**TRABB-A** : Audio Leitung A (Entspricht UIC568 Leitung 3)
**TRABB-B** : Audio Leitung B (Entspricht UIC568 Leitung 4)

### 2-Draht Wahlleitung (TRABB-WL)
Für die Wahl der Zieldestination und zur Signalisation der Zustände wird eine verdrillte 2-Draht Leitung verwendet.

Zur Wahrung der Kompatibilität zu der UIC568, ist die Signalisation mit der UIC Leitung 5/6 oder 7/8 elektrisch kompatibel. Anders als beim Aderpaar TRABB-AB, darf kein anderes Gerät angeschlossen werden, dass nicht zur TRABB Spezifikation kompatibel ist.

#### Elektrische Eigenschaften
**Vlow**: < 9 VDC
**Vhigh**: > 16 VDC
**Vhysterese**: 7 VDC
**Vmax**: 37 VDC
**Zin-min**: 560 kOhm

Die Spannungen sind zur Leitung TRABB-L referenziert. Die minimale Impedanz auf TRABB-WL beträgt 2.2 kOhm.

#### Leitungsnamen
**TRABB-W** : Wahlleitung W
**TRABB-L** : Wahlleitung L

#### TRABB Disable
Das TRABB System kann ausgeschaltet werden, falls die primäre Kommunikation über Ethernet erfolgt. In diesem Fall kann die Leitung TRABB-WL konstant auf logisch "High" gesetzt werden. Damit wird verhindert, dass ein Gerät eine Rufnummer wählen kann. Bei Ausfall des Ethernet oder bei einem Lastabwurf kann durch die Zugssteuerung die Leitung TRABB-WL auf logisch "Low" gesetzt werden und damit der Backup Modus aktiviert werden. Alternativ kann eine entsprechende priorisierung und detektion der Ethernet Verbindung durch die Gerätesoftware erfolgen.

### 2-Draht PA-Kontrollleitung (TRABB-RS) (optional)
Sind mehrere Audioverstärker pro Wagen vorhanden, sind diese als Slave an einen Master, mittels TRABB-RS und TRABB-PQ zu konnektieren, sofern die maximale Anzahl Verstärker, inkl. möglichen Traktionen, 16 übersteigt.

Die Aufgabe der PA-Kontrollleitung (TRABB-RS) ist das aktivieren der PA Ausgabe auf den Slavegeräten.

#### Elektrische Eigenschaften
**Vpa-on**: < 9 VDC
**Vpa-off**: > 16 VDC
**Vhysterese**: 7 VDC
**Vmax**: 37 VDC

Die Spannungen sind zur Leitung TRABB-S referenziert.

### 2-Draht PA-Audioverteilung (TRABB-PQ) (optional)
Zur In-Wagen verteilung des PA-Signals, wird ein Audio Output des Masters, mit einem Audio-Eingang des Masters und allen im Wagen vorhandenen Slaves, verbunden. Somit ist die Konsistenz der Audio Level gewährleistet. Das anliegende Signal wird nur an die Lautsprecher ausgegeben, falls das Signal TRABB-RS aktiv ist.

#### Elektrische Eigenschaften
Professional audio Line Level
**Nominal**: +4 dBu

## Addressierung
Zur Anwahl eines Empfängers und Identifikation des Absenders werden IDs vergeben. Es ist zu beachten, dass wegen der Verwendung eines Pulswählverfahrens die niedrigen Nummern zu bevorzugen sind, da weniger Pulse nötig. Siehe dazu Kapitle "Wahlzahlen".

### Endgerät (Device-ID)
Jedes Endgerät verfügt über eine im jeweiligen Zug eindeutige ID mit exakt 3 Ziffern, die NICHT mit einer Ziffer 0 beginnen darf. Breich 100 - 999.

### Gruppen (Group-ID)
Zur Anwahl ganzer Gruppen sind in der Gerätekonfiguration die bildung von Gruppen IDs möglich. Folgend die präferierten Nummern für vordefinierte Empfänger. Die ID Länge beträgt exakt 3 Ziffern und beginnen zwingend mit einer Ziffer 0. Bereich 000 - 099.

#### PA Gruppen
Bereich 000 - 049

#### Intercom Gruppen
Bereich 050 - 099

```
Wagen-PA '011' : Ansage in dem Wagen, in dem sich der Sender befindet.
Zug-PA '012' : Ansage im jeweiligen Zug in dem sich der Sender befindet.
Traktion-PA '013' : Ansage in der ganzen mehrteiligen Traktion.
Cab '051' : Addressiert den Lokführer.
Zub '052' : Addressiert alle Zugsbegleitersprechstellen im Zug.
Pers '053' : Addressiert Cab und Zub.
Zugsfunk '054' : Addressiert die Leitzentrale.
```

### Zugsnummer (Train-ID)
Damit es in keinem Fall zu ID-Kollisionen kommt bei der komposition einer Mehrfachtraktion, muss jeder Zug über eine eindeutige ID verfügen. Die ID kann aus einer freien Folge von Nummern bestehen, die möglichst kurz zu halten ist. Die Zugsnummer darf NICHT 0 sein.

#### Format
`<N><ID>`
`<N>` : ID-Länge (Anzahl Ziffern der nachfolgenden Zugsnummer ID)
`<ID>` : Zugsnummer (Eindeutig innerhalb einer Traktion)

## Rufnummern
Die Rufnummer setzt sich wie folgt zusammen:
`<Caller-ID><Destinaion-ID><PS>`
Dabei setzt sich die Caller-ID und Destinaion-ID wiederum wie folgt zusammen:
`<Train-ID><Device-ID>`
Die Prüfsumme PS wird durch die XOR verknüpfung aller vorhergenden Zahlen errechnet. Das Resultat wird mit Modulo 10 auf in jedem Fall 1 Stelle normalisiert.
`PS = (Rufnummer[0] XOR Rufnummer[1] ... XOR Rufnummer[n]) % 10`

Wird eine Gruppenfunktion für eine Mehrfachtraktion aufgeruffen, z.B. die Traktion-PA, wird die Train-ID durch die fixe Nummer '10' ersetzt (Wildcard).

### Rufnummernbeispiele
**Zug-1**: Zugnummer 3456. Diese Nummer ist 4 Stellig und die Train-ID ist demzufolge 43456.
**Zug-2**: Train-ID 46689.
**Zub-1**: Die Zugsbegleitersperechstelle hat die Device-ID 123
**Zub-2**: Device-ID 124
**PEI-1**: Notsprechstelle mit Device-ID 125

#### Beispiel 1 - Mehrfachtraktion PA
Es soll von der Zugsbegleitersprechstelle Zub-1 eine Durchsage auf der ganzen Mehrfachtraktion gemacht werden. Die Zub-1 befindet sich im Zug-1.

Rufnummer: '43456 123 10 013 x' // Todo: Prüfsumme x in den Beispielen berechnen.

#### Beispiel 2 - Intercom
Ein in Not geratener Fahrgast möchte einen Notruf über die PEI-1 im Zug-1 durchführen. Das System ist so konfiguriert, dass der Lokführer und die Zugsbegleiter addressiert werden sollen.

Rufnummer: '43456 125 10 053 x'

#### Beispiel 3 - Zub/Zub
Der Zugsbeglieter möchte eine Verbindung zur Zub-2 im Zug-1 mit einer Zugsbeglietersprechstelle Zub-1 im Zug-1 aufbauen.

Rufnummer: '43456 123 43456 124 x'

## Wahlverfahren - PA

### Prüfung Leitung frei
Bevor mit der Wahl eines Empfängers begonnen werden kann, muss vom Gerät geprüft werden ob die TRABB-WL für mindestens 400 ms kein Logisch "High" auftritt. Dies soll verhindern, dass zwei Geräte gleichzeitig als Master die Leitung benutzen.

### Wahl der Rufnummer
War die Kollisionskontrolle auf der Leitung TRABB-WL erfolgreich, kann mit dem Absetzen der Rufnummer auf TRABB-WL gestartet werden. Dabei werden die abgesetzten Impulse vom Sender zurückgelesen. Sind die abgesetzten und eingelesenen Werte identisch, war die Wahl gültig.

#### Abbruchbedingungen und Verhalten
**Die Leitung ist mehr als 400 ms konstant logisch "High"**: Ein Gerät/Funktion/Fehler mit höherer Priorität beansprucht die Leitung für sich. Die Wahl der Rufnummer wird abgebrochen. Es folgt ein standby zwischen random 0.5 und 3 Sekunden. Die erneute Wahl beginnt wieder mit dem Abschnitt "Prüfung Leitung frei". Es sind maximal 3 Versuche zulässig.

**Die eingelesene Impulsfolge entspricht nicht der abgesetzten**: Dies entspricht einer Kollision, mit warscheinlich quasi gleichzeitigem Beginn der Rufnummerwahl durch zwei oder mehr Geräte. Die Leitung TRABB-WL wird vom fehlererkennenden Sender für 500 ms auf logisch "High" gesetzt. Es folgt ein standby zwischen random 0.5 und 3 Sekunden. Die erneute Wahl beginnt wieder mit dem Abschnitt "Prüfung Leitung frei". Es sind maximal 3 Versuche zulässig.

### Steuerung Durchsage
Konnte die Rufnummer ohne Kollision abgesetzt werden, müssen der Sender und die Empfänger ihre 600 Ohm Audio-Transformatoren auf die Leitung TRABB-AB verbinden. Der Sender muss innerhalb von 400 ms mit dem absetzen einer konstanten Ziffernfolge der Ziffer '1' auf der Leitung TRABB-WL beginnen um die Durchsage zu aktivieren. Die Pause von 200ms nach der Übermittlung der letzten Rufnummerziffer muss eingehalten werden. Sobald die Leitung TRABB-WL durch den Sender auf logisch "Low" gesetzt wird, ist die Durchsage beendet. Aus Sicherheitsgründen, kann in der Gerätekonfiguration eine maximale Durchsagedauer aktiviert werden. Die Prüfsumme wird von den Empfängern bei PA Funktionen nicht überprüft und ignoriert.

#### Abbruchbedingungen und Verhalten
**Übersteuerung**: Die Zugssteuerung oder ein anderes entsprechend konfigurierte Gerät, kann durch setzen der Leitung TRABB-WL auf logisch "High" für minimum 500ms, die Übertragung beenden.

**Ablauf maximale Durchsagedauer**: Sender bricht Übertragung der Durchsage mit setzten der Leitung TRABB-WL auf logisch "Low" ab.

## Wahlverfahren - Intercom

### Prüfung Leitung frei
Bevor mit der Wahl eines Empfängers begonnen werden kann, muss vom Gerät geprüft werden ob die TRABB-WL für mindestens 400 ms kein Logisch "High" auftritt. Dies soll verhindern, dass zwei Geräte gleichzeitig als Master die Leitung benutzen.

### Wahl der Rufnummer
War die Kollisionskontrolle auf der Leitung TRABB-WL erfolgreich, kann mit dem Absetzen der Rufnummer auf TRABB-WL gestartet werden. Dabei werden die abgesetzten Impulse vom Sender zurückgelesen. Sind die abgesetzten und eingelesenen Werte identisch, war die Wahl gültig.

#### Abbruchbedingungen und Verhalten
Die Leitung ist mehr als 400 ms konstant logisch "High": Ein Gerät/Funktion/Fehler mit höherer Priorität beansprucht die Leitung für sich. Die Wahl der Rufnummer wird abgebrochen. Es folgt ein standby zwischen random 0.5 und 3 Sekunden. Die erneute Wahl beginnt wieder mit dem Abschnitt "Prüfung Leitung frei". Es sind maximal 3 Versuche zulässig.

Die eingelesene Impulsfolge entspricht nicht der abgesetzten. Dies entspricht einer Kollision, mit warscheinlich quasi gleichzeitigem Beginn der Rufnummerwahl durch zwei oder mehr Geräte. Die Leitung TRABB-WL wird vom fehlererkennenden Sender für 500 ms auf logisch "High" gesetzt. Es folgt ein standby zwischen random 0.5 und 3 Sekunden. Die erneute Wahl beginnt wieder mit dem Abschnitt "Prüfung Leitung frei". Es sind maximal 3 Versuche zulässig.

### Klingeln
Konnte die Rufnummer ohne Kollision abgesetzt werden, müssen der Sender und der Empfänger ihre 600 Ohm Audio-Transformatoren auf die Leitung TRABB-AB verbinden. Der Anruf wird vom Empfänger, nach prüfen der Rufnummernprüfsumme, durch das setzten der Leitung TRABB-WL auf logisch "High" bestätigt. Dies verhindert eine Wahl eines anderen Gerätes. Die Verbindung der Audio-Transformatoren und die Klingelfunktion, müssen in einem Zeitfenster von maximal 400 ms, nach der Übertragung der letzten Rufnummerziffer, erfolgen. Die Klingeldauer ist in der Gerätekonfiguration zu definieren, beträgt standardmässig 2 min.

#### Abbruchbedingungen und Verhalten
**Verzögerte Rufannahme**: Wird das Gespräch nach beendigung der maximalen Klingeldauer nicht durch den Empfänger angenommen, ist das Gespräch unverzüglich abzubrechen. Sender und Empfänger entfernen ihren Audio-Transformator von der Leitung TRABB-AB. Die Leitung TRABB-WL geht zurück auf logisch "Low". Es folgt ein standby zwischen random 0.5 und 3 Sekunden. Die erneute Wahl beginnt wieder mit dem Abschnitt "Prüfung Leitung frei". Es sind maximal 3 Versuche zulässig.

**Gerät wird nicht angesprochen**: Antwortet das Gerät nicht innerhalb der geforderten 400 ms mit einem Klingelsignal auf Leitung TRABB-WL. So gilt der Empfänger als momentan nicht erreichbar. Es folgt ein standby zwischen random 0.5 und 3 Sekunden. Die erneute Wahl beginnt wieder mit dem Abschnitt "Prüfung Leitung frei". Es sind maximal 3 Versuche zulässig.

**Prüfsumme nicht korrekt**: Siehe "Gerät wird nicht angesprochen".

### Annahme des Gesprächs
Der Anruf wird von einem Zugsbegleiter z.B. durch drücken einer Taste an einer Intercom angenommen. Die gewählte Intercom ist nun der Master für den restlichen Verlauf des Anrufs. Der Master setzt ein DTMF Signal auf der Leitung TRABB-AB ab und signalisiert den anderen möglichen Empfängern die Wahl des Masters. Die restlichen möglichen Empfänger trennen so bald möglich ihren Audio-Transformator von der Leitung TRABB-AB und beenden die Ausgabe eines Klingeltons. Nach absetzen des DTMF Signals beginnt die Übertragung einer konstanten Ziffernfolge '1' auf der Leitung TRABB-WL. Durch den Wechsel des Signals auf TRABB-WL von konstant "High" auf die konstante Ziffernfolge '1' wird das Gespräch mit dem Sender aufgebaut. Das Gespräch wird durch den Master, durch setzen der Leitung TRABB-WL auf logisch "Low" beendet.

#### Definition DTMF Signal
TODO: DTMF Signal spezifizieren.

#### Abbruchbedingungen und Verhalten
Die Leitung TRABB-WL ist nach absetzen des DTMF Signals, mehr als 400 ms konstant logisch "High": Ein Gerät/Funktion/Fehler mit höherer Priorität beansprucht die Leitung für sich. Das Gespräch wird augenblicklich abgebrochen. Sender und Empfänger entfernen ihren Audio-Transformator von der Leitung TRABB-AB. Keine Wiederwahl.

### Abbruch des Gesprächs
Der Empfänger bricht das Gespräch mit dem setzten von logisch "High" auf TRABB-WL für minimum 500ms ab. Der TRABB ist frei für ein erneutes Gespräch. Keine Wiederwahl.

## PA-Erweiterung
Ist mehr als ein Verstärker pro Wagen vorhanden, soll ein Gerät als Master und die anderen Verstärker als Slave definiert werden. Dies muss in der Konfigurationsdatei der Geräte entsprechend gesetzt werden. Der Master wird mit den Slaves über die Verbindungen TRABB-RS und TRABB-PQ verbunden. Siehe dazu Abschnitt "2-Draht PA-Kontrollleitung (TRABB-RS) (optional)" und "2-Draht PA-Audioverteilung (TRABB-PQ)".

### Aktivieren der Beschallung (PA)
Durch setzen der Leitung TRABB-RS auf logisch "High" wird das an der Leitung TRABB-PQ anliegende Signal an den Lautsprecher ausgegeben. Das Signal auf TRABB-PQ wird dabei vom Master erzeugt und über die 2-Drahtleitung an die Slaves verteilt. Nur der Master darf die Leitung TRABB-RS aktiv ansteuern.

## Notes
Die hohe Eingangsimpedanz der Signaleingänge für die Leitung TRABB-WL kann z.B. mit einem Emittergekoppelten Schmitt-Trigger erfolgen, der Seinerseits einen Optokoppler steurt. Die 24 VDC für die Eingangsschaltung steht ohnehin Primärseitig für die Signalisation zur Verfügung. Der Eingang des Schmitt-Trigger kann auch als Darlington ausgeführt werden. Die Schaltung ist auf Störeinflüsse zu testen. Ein Eingang direkt ausgeführt als Optokoppler oder Relay ist nicht zulässig.

Durch die Übermittlung der Caller-ID vor dem Rufziel Destinaion-ID, kann eine Kollision frühzeitig erkannt werden. Somit wird ausgeschlossen, dass zwei Geräte die gleiche Destinaion-ID gültig anwählen. (Idee von Samir)
