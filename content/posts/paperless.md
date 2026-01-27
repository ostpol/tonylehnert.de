+++
title = 'Mein Setup fürs papierlose Büro'
date = 2021-01-03T17:31:41+01:00
draft = false
+++

In den letzten Jahren habe ich immer wieder darüber nachgedacht, wie ich mit Papier umgehen möchte. Es stapelt sich, bestenfalls in Ordnern, kann kein Ctrl+F und nimmt Platz weg. Ansätze fürs papierlose Büro gibt es viele, aber bisher wollte für mich nichts (dauerhaft) funktionieren. Manchmal war es der langsame Scanner, der nicht duplex scannt, manchmal war es die Organisation der Dateien.

Irgendwann bin ich dann über einen gebrauchten Fujitsu ScanSnap S500M (Yay, duplex!) gestolpert und habe ein Setup gefunden, das für mich funktioniert. Mittlerweile sollten für mein Setup auch Scan-Apps mithalten können, solange sie die Scans zentral ablegen können. Duplex machen die aber immer noch nicht…

Automatismen!
-------------

Am meisten habe ich mich davor gedrückt einfach anzufangen. Der bereits entstandene Papierhaufen muss initial gescannt werden. Das geht deutlich einfacher und schneller, wenn man den Scanvorgang so weit wie möglich automatisiert.

Der Scanner hängt bei mir an einem Raspberry Pi Model B Rev 1 (Ja, wirklich Rev 1 und er reicht vollkommen!) mit Raspbian 10 Lite. Bei [Kevin Liu](https://kliu.io/post/automatic-scanning-with-scansnap-s500m/) habe ich ein Script gefunden, das das Scannen übernimmt und, natürlich mit kleinen Anpassungen auf meine Umgebung, auch bei mir gut funktioniert. Auf scanbd verzichte ich. Das Script starte ich über ein Webinterface und MQTT (danke [Node-RED](https://nodered.org/) und [mqtt-launcher](https://github.com/jpmens/mqtt-launcher)). Damit könnte ich sogar Alexa und Siri bitten einen Scan zu starten. 🤔 Damit das Script seinen Dienst verrichten kann, müssen noch drei Abhängigkeiten installiert werden:

*   sane-utils
*   ghostscript
*   imagemagick

In meinem Fall wird das erzeugte PDF auf einem smb-Share meines NAS abgelegt. RAID1 lässt mich bei manchen Dokumenten etwas ruhiger schlafen. Irgendwann sollte ich wohl noch für einen Spiegel außerhalb sorgen.

Abheften
--------

Auf meinem NAS existiert eine Verzeichnisstruktur:

*   documents
    *   inbox
    *   store
    *   archive

Das ist die Grundlage für [Paperless](https://github.com/the-paperless-project/paperless), das ab hier in einem bzw. zwei Docker Containern die Arbeit übernimmt.

Das gescannte Dokument landet in /documents/inbox und wird dort vom document\_consumer entgegengenommen, verarbeitet und von /documents/inbox nach /documents/store geschoben. Dort liegt das PDF und ein Thumbnail. Der zu Paperless gehörende Webserver kümmert sich um das Webinterface.

Dranbleiben
-----------

Wenn der erste Schritt gemacht ist und nur noch die wichtigsten Dokumente in Ordnern herumhängen, muss man „nur noch“ dranbleiben. Das funktioniert bei mir nun immerhin schon seit einem Jahr. 🥳

Update 2021-01-23
-----------------

Jemand hat [paperless-ng](https://github.com/jonaswinkler/paperless-ng) gebaut. Auf den ersten Blick und so weit ich das einschätzen kann, ist der Unterbau etwas moderner, flexibler und vielleicht auch etwas performanter. Das Webinterface hat ein Update bekommen. Das sollte man im Auge behalten.