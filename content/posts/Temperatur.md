+++
title = 'iOS Widget für die Temperatur auf dem Sonnenberg'
date = 2021-04-06T16:39:33+01:00
draft = false
+++

Seit einiger Zeit logge ich die Außentemperatur an unserer Wohnung auf [temperatur.tonylehnert.de](http://temperatur.tonylehnert.de/). Zuerst in Altchemnitz, jetzt auf dem Sonnenberg.

Wenn die Daten also schon im Internet herumliegen, kann ich mit [Scriptable](https://scriptable.app/) für iOS auch ein Widget bauen. Ich bediente mich eines [Grundgerüsts](https://hackernoon.com/how-to-create-a-slick-ios-widget-in-javascript-e11p33t2) und passte es optisch [diesem Widget](https://gist.github.com/kevinkub/46caebfebc7e26be63403a7f0587f664) für die Corona-Inzidenz an (bzw. einer alten Version davon).

> Die Version des Inzidenz-Widgets, die bei mir noch lief und auf die ich das Temperatur-Widget angepasst habe, war offensichtlich uralt. Egal, reicht so. [pic.twitter.com/tlIKOPUW9w](https://t.co/tlIKOPUW9w)
> 
> — Tony (@ostpol) [April 6, 2021](https://twitter.com/ostpol/status/1379462391684022273?ref_src=twsrc%5Etfw)

Mein Code ist wie immer nicht sauber und bedarf sicher der ein oder anderen Optimierung – tut das bitte, wenn ihr mögt und gebt auf Twitter Bescheid, damit ich das hier einbauen kann (ich hör‘ GitHub leise rufen). Die Datenbank wird in einem Intervall von 15 Minuten aktualisiert. Der Refresh des Widgets wird von iOS gesteuert, das Intervall liegt wohl irgendwo zwischen 4 und 7 Minuten. Die Aktualität der Daten hängt außerdem davon ab, dass der Sensor zuverlässig ausgelesen wird, ich hier nichts zerstöre und das Internet nie ausfällt.

Wer Scriptable noch nicht kennt:

*   Scriptable aus dem AppStore [installieren](https://apps.apple.com/de/app/scriptable/id1405459188)
*   Code fürs Widget kopieren, neues Script in Scriptable erstellen und Code einfügen

```
const dataUrl = "https://temperatur.tonylehnert.de/ios_widget.php";

let widget = await createWidget();
widget.setPadding(15, 14, 14, 14)
Script.setWidget(widget);
widget.presentSmall();
Script.complete();

async function createWidget() {
    const widget = new ListWidget();

    const data = await new Request(dataUrl).loadJSON();

    //Header
    let titleRow = widget.addText("🌡️ Temperatur".toUpperCase());
    titleRow.font = Font.mediumSystemFont(13)
    widget.addSpacer();

    //Temperature data
    let temperatureRow = widget.addText(`${data[0].temp}°C`);
    temperatureRow.font = Font.boldSystemFont(24)
    temperatureRow.textColor = data[0].temp <= -23 ? new Color("cc0199") : data[0].temp <= -18 ? new Color("980299") : data[0].temp <= -12 ? new Color("7030a2") : data[0].temp <= 0 ? new Color("3432ff") : data[0].temp < 4 ? new Color("0199fe") : data[0].temp >= 4 ? new Color("00af50") : data[0].temp >= 10 ? new Color("92d14e") : data[0].temp >= 16 ? new Color("ffc100") : data[0].temp >= 21 ? new Color("f79649") : data[0].temp >= 27 ? new Color("fe0002") : data[0].temp >= 32 ? new Color("cc0001") : data[0].temp >= 38 ? new Color("c10001") : Color.black();

    //location
    let locationRow = widget.addText("Sonnenberg");

    return widget;
}
```


*   Ein neues Scriptable-Widget auf dem Home Screen erstellen und das Temperatur-Script als Quelle wählen