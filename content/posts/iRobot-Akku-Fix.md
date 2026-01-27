+++
title = 'Troubleshooting: iRobot Roomba i7+ lädt nicht mehr'
date = 2022-01-05T19:08:09+01:00
draft = false
+++

Mein iRobot Roomba i7+ hat nach nahezu exakt 12 Monaten den Dienst quittiert. Nach über 12 Stunden auf der Ladestation war der Akku immer noch leer. Hier möchte ich zusammenfassen, was getan werden kann, um den Fehler zu finden.

![](https://i.redd.it/4m46m7zgpn881.jpg)

Sind die Ladekontakte sauber?
-----------------------------

iRobot empfiehlt in diesem Fall die Säuberung der Ladekontakte an Roboter und Ladestation. Am besten soll das mit einem „Schmutzradierer“ funktionieren. Welche Kontakte gemeint sind und wie sie gereinigt werden erklärt iRobot [hier](https://support.irobot.de/s/article/528). Das brachte in meinem Fall keine Hilfe.

Reboot tut gut.
---------------

Die Ladestation lässt sich durch das Trennen der Stromversorgung neu starten. Der Roboter startet neu, wenn man die CLEAN-Taste für mind. 20 Sekunden gedrückt hält. Damit konnte ich mir auch nicht helfen.

Nimm ihm die Energie!
---------------------

Als nächstes habe ich die Batterie gezogen und wieder gesteckt. Hat auch nichts gebracht, schadet aber auch nicht. Wie man an die Batterie kommt, beschreibt [iFixit](https://de.ifixit.com/Anleitung/iRobot+Roomba+i7+Battery+Replacement/131450?lang=en).

Tut die Ladestation was sie soll?
---------------------------------

Wenn der Roboter nicht schuld ist, ist es vielleicht die Ladestation? Mit einem Multimeter lässt sich prüfen, ob die Ladespannung passt:

Ohne den Roboter geben die Ladekontakte einer funktionierenden Station etwa 2,8V aus. Mit Roboter 20,3-20,6V. Um mit Roboter zu messen, habe ich an jeden Ladekontakt ein Kabel geklemmt, etwas fummelig, funktioniert aber. Die Spannung passte in meinem Fall.

Bleibt die Leistung. Auf Reddit wurde ich darauf aufmerksam gemacht, dass sich eine funktionierende Station während des Ladens >20W genehmigt. Mit einem Wattmeter habe ich gemessen und nur 2-6W Leistung festgestellt. Ein gutes Indiz für eine defekte Ladestation. Wer sich an eine Reparatur wagen möchte, bekommt hier einen Hinweis:

Nun haben wir im Haus einen weiteren i7+, also konnte ich mir eine nachweislich funktionierende Ladestation leihen. Auch diese leistete nur 2-6W. Das war’s also auch nicht.

Factory Reset 🙄
----------------

So blieb mir noch der Factory Reset. Nervig, weil der Roboter die Wohnung kennt und nicht vergessen soll. Ist aber halb so schlimm, denn die iRobot Home App fragt vor dem Reset, ob die vorhandene Smart Map in der Cloud zwischengespeichert werden soll. Hab ich gemacht, nur laden wollte der Roboter nicht. Ich habe die Chance allerdings genutzt und den kolonial anmutenden Roboternamen „Sam“ in „Dirty Hairy“ geändert. \*badumtss\*

Ist es doch die Batterie?
-------------------------

Nach all dem konnte es eigentlich nur noch die Batterie sein. Ich steckte Büroklammern in die Pins der Batterie und maß eine Spannung von 1,3mV. Ein sehr eindeutiges Indiz für einen defekten Akku.

![](https://tonylehnert.de/wordpress/wp-content/uploads/2022/01/image.png)

Ich bestellte also eine neue Batterie. Und weil ich iRobot keine 59€ für einen Akku in den Rachen werfen wollte, der nach 12 Monaten die Hufe hebt, habe ich mich für das günstigere Alternativmodell von Green Cell entschieden. Bonus: Der 3rd party Akku hat 800mAh mehr Kapazität. Nach Lieferung der Batterie verbaute ich sie und stellte den Roboter auf die Ladestation, der Akku war vorgeladen und wurde anstandslos voll aufgeladen. 🎉

Was ich in einer Woche mit manuellem Staubsaugen gelernt habe? Zwei Katzen und ein Hund produzieren echt viel Fell.