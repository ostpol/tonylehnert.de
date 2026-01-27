+++
title = 'Smartmeter auslesen mit ESP8266 und SMLReader (und MQTT)'
date = 2021-01-21T17:54:04+01:00
draft = false
+++

[Bis 2032](https://www.bundesnetzagentur.de/DE/Sachgebiete/ElektrizitaetundGas/Verbraucher/Metering/SmartMeter_node.html) sollen alle Verbraucher eine moderne Messeinrichtung (mME) bekommen. In Chemnitz verteilt eins energie/inetz offensichtlich schon fleißig mMEs. So hatte ich schon in unserer vorherigen Wohnung ein Smartmeter und auch in der neuen Wohnung hängt so ein Teil im Keller. In meinem Fall sind beide Geräte vom Typ [DWS 7412.1](https://www.dzg.de/fileadmin/dzg/content/downloads/produkte-zaehler/dvs74/dzg_dvs74_handbuch.pdf).

Die meisten mMEs lassen sich über eine optische Schnittstelle auslesen. Weit verbreitet sind hier wahrscheinlich fertige USB-Leseköpfe. Wer einen Lötkolben halten kann, kann sich aber auch selbst einen Lesekopf bauen. Ich habe mir vor einiger Zeit ein Gehäuse hierfür konstruiert und gedruckt. Auf [Thingiverse](https://www.thingiverse.com/thing:3844844) habe ich auch die Quellen für die Elektronik verlinkt.

Auf Softwareseite ist [volkszaehler.org](https://www.volkszaehler.org/) schon lange dabei und wird gern genutzt. Auch ich habe damit zuerst die Werte vom Zähler abgeholt, allerdings hat mich die Komplexität (hätte ich mich intensiv damit beschäftigt, wäre das sicher kein Problem gewesen) und die Notwendigkeit eines Rechners (ja, ein Raspberry Pi tuts auch) immer etwas gestört.

Irgendwann bin ich dann auf den [SMLReader](https://github.com/mruettgers/SMLReader) von Michael Rüttgers aufmerksam geworden. Fertiger Code für ESP8266 Boards, der die Werte des Zählers an einen MQTT-Broker schickt. 🎉

Bei mir hängt seitdem ein NodeMCU am Selbstbau-Lesekopf (bisher über 15 Meter 2×0,75mm2 verbunden) und meldet mir den momentanen Verbrauch und den Gesamtzählerstand. In der neuen Wohnung reichen 15 Meter nicht mehr aus und ich versuche den Sensor über das noch deutlich längere Cat7-Kabel zwischen Wohnung und Keller anzuschließen. Funktioniert das nicht, brauche ich wohl Hardware mit Ethernet (und PoE, bitte?) im Keller, die kein Raspberry Pi ist. **Update:** Der Sensor ist über die Gebäudeverkabelung mit dem NodeMCU verbunden und funktioniert. Offensichtlich ist die furchtbar einfache Elektronik recht genügsam.