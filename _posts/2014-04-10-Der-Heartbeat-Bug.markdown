---
layout: post
title:  "Der (wide)openSSL-Heartbleed-Bug oder: ändere alle Passwörter und Keys ... jetzt"
date:   2014-04-10 14:15:49
categories: sicherheit heartbleed
---

### Yüahh, das Internet ist kaputt!
So begab es sich am "Sun, 1 Jan 2012 00:59:57 +0200", also zu einer Zeit, zu welcher man wohl niemandem vorbehaltslos unterstellen mag stocknüchtern zu sein, dass Dr. Stephen Henson den [Code][gitdiff] von Robin Seggelmann ins openSSL-Repo pusht.
Knapp 2½ Jahre später [twittert][tweet] Neel Mehta: "Openssl 0day, may expose session data from servers / clients. Patch your stuff: [http://www.openssl.org/news/secadv\_20140407.txt][advi]", was weltweit (gemeint ist das [Heise-Forum][heisef]) die größte Massenpanik seit 9/11 auslöste. Völlig zurecht! Und so haben die [oberen 1000][top1000] zumeist auch artig alle verfügbaren [Patches][patchdiff] eingespielt. Der macht den openSSL-Code nicht unbedingt schöner, aber Schönheit und Spaghetti waren schon immer zwei Paar Schuhe.

Was für die wichtigen 1000 gilt muss aber nicht unbedingt auch für die noch wichtigeren übrigen 4 Milliarden (grob über die Anzahl der verballerten IPv4 Adressen und meinen Daumen gepeilt) gelten. Für durchschnittlich paranoide Computerbenutzer muss jedes System, das im Zeitraum seit Januar 2012 mit den betroffenen openSSL-Versionen gesichert wurde, als kompromittiert gelten. Klingt dramatisch, ist aber noch viel schlimmer. Beispiel gefällig?

### bobSSL hat Heartbleed und es trifft alle
Alice betreibt einen Webshop, hantiert dort mit allerhand persönlichen Daten und den unglaublich sicheren Passwörtern (die sind so sicher, die kann man für alles nehmen!) ihrer Kunden herum. Damit jeder sieht wie vertrauenswürdig ihr Shop ist, klickt sie sich ein SSL-Zertifikat beim Anbieter bobSSL.  
Der Anbieter ist besonders gut für Alice geeignet, weil alles ganz easy über den Browser läuft, selbst der private Key (doch, das gibt's!). Blöd nur, dass bobSSL eine kaputte openSSL-Version benutzt und seit Monaten von Charlie dem Schelm abgeschnorchelt wird. Alice's SSL ist also kompromittiert, obwohl sie selbst niemals eine eine der vom Heartbleed-Bug betroffende openSSL-Version verwendet hat.  
Daraus ergibt sich, dass alle Kennwörter (die sind so sicher, die kann man für alles nehmen!!1elf!) ihrer Kunden verbrannt sind und somit alle E-Mail-Konten, Paypal- und Facebook-Accounts der Kunden von Alice als kompromittiert gelten dürfen. Und von der NSA habe ich noch gar nicht angefangen!

### Nein, Du warst nicht schnell genug
Kurz nachdem die Katze aus dem Sack war gab es den ersten [Code][heartbleeder] zum "Testen ob man selbst betroffen ist". Daraus wurde schnell ein [Exploit der irgendwie läuft][exploit1]. Das gleiche gab's später dann [nochmal in schön][exploit2] und wer was ernsthaftes brauchte, wartete einfach auf das [Metasploit Module][msf]. Irgendwann später wurde es in Mitteleuropa hell.  
Selbst wer ganz fest daran glaubt (bitte,bitte,bitte), dass vor dem Public Disclosure niemand die Lücke kannte muss einsehen, dass es in China schon seit Stunden Gehacktes gab als in Schland grade die Kaffeemaschinen hochgefahren wurden.

### Krisenmanagement, nein Danke!
Wärend Alice sich ganz vorbildlich neue Keys an ihre kompromottierte web.de-Addresse schicken ließ (der Shop ist wichtiger als das E-Mail-Kennwort zu ändern, den Kunden zuliebe!), haben manche den Knall anscheinend noch immer nicht gehört.  
Soeben erhielt eine Mail von einem guten Freund. Er lebt in einem rechtfreien Raum auf einer Insel im Südpazifik und hat sich den Spaß erlaubt ein /16-Netz nach verwundbaren Webserver-Installationen abzuklappern. Ziel waren Root-Server im Rechenzentrum einer deutschen Internet-Hosting-Gesellschaft. Dies sind seine Ergebnisse:

#### > 10% aller erreichbaren Systeme noch immer verwundbar

{% highlight ruby %}
# 10.04.2014 ca. 12.00 Uhr

Anzahl IP's:         65534
Port 443 offen:       5326
Port 443 zu:          4080
Port 443 gefiltert:   4930
Verwundbare Systeme:   562
{% endhighlight %}

Das sind "nur" die Webserver. SMTPS, POP3S, IMAPS und wie sie alle heißen würden das Ergebnis nach oben korrigieren. 

Wer meint, dass auf solchen Servern nur belanglose  Homepages und Blogs liegen täuscht sich gewaltig. Mittlerweile ist so ziemlich jedes Stück Computer über ein Webinterface erreichbar. Hier geht es um Interfaces für E-Mail, Datenbanken, Bug-Tracking und Support-Systeme. Es geht um das was viele Unternehmen zurecht als ihre Kronjuwelen betrachten können (zumindest sobald es weg ist :-)).

Den Heartbleed-Bug kann man gar nicht ernst genug nehmen. Warum nicht in jeder Nachrichtensendung daruf hingewiesen wird, dass **alle** Passwörter zu ändern sind verstehe wer will. Wer glaubt, dass nichts passiert (ist), nur weil niemand etwas bemerkt (hat), hat noch nicht verstanden was es heißt in der Zeit nach Snowden zu leben.


[msf]: https://github.com/rapid7/metasploit-framework/blob/master/modules/auxiliary/scanner/ssl/openssl\_heartbleed.rb
[exploit2]: https://gist.github.com/takeshixx/10107280
[exploit1]: https://github.com/trapp/heartbleeder
[heartbleeder]: https://github.com/titanous/heartbleeder
[heisef]: http://www.heise.de/security/news/foren/S-Der-GAU-fuer-Verschluesselung-im-Web-Horror-Bug-in-OpenSSL/forum-277761/list/
[top1000]: https://github.com/musalbas/heartbleed-masstest/blob/master/top1000.txt
[advi]: http://www.openssl.org/news/secadv_20140407.txt
[tweet]: https://twitter.com/neelmehta/status/453255264938901504
[gitdiff]: http://git.openssl.org/gitweb/?p=openssl.git;a=commitdiff;h=4817504d069b4c5082161b02a22116ad75f822b1
[patchdiff]: http://git.openssl.org/gitweb/?p=openssl.git;a=commitdiff;h=96db9023b881d7cd9f379b0c154650d6c108e9a3
