# Installeer Perl en setup Apache op Mac OS X 10.6.8 VM

![Perl afbeelding](images/perl.jpg)

## Vooraf

Zoals jullie wel zullen weten uit vorige posts ben ik geïnteresseerd in een **webdev** omgeving op een oude **Xserve** server. Specifiek wil ik **Perl CGI** scripts kunnen ontwikkelen op **Mac OS X 10.6.8**. Het is sowieso misschien een slecht idee om **Perl CGI** anno 2025 nog te ontwikkelen, maar tja dat is nu eenmaal wat ik wil doen.

Op die **webdev** server kan ik dan ook **HTML/CSS** en **vanilla javascript** ontwikkelen als **frontend** en dus **Perl CGI** als backend.

Ik heb al een artikel geschreven over de installatie van **Perl** op **Mac OS X 10.6.8** wat [hier](https://github.com/gwendebrul/blog/tree/main/2024/008%20newer%20perl%20on%20macosx-10-6-8) te lezen valt.

Maar sindsdien heb ik niet stil gezeten en een paar aanpassingen gedaan, en die aanpassingen zijn in dit artikel te lezen.

In dit artikel ga ik dus een nieuwere versie van **Perl** installeren en gebruik ik de ingebouwde **Apache** server als **webserver**. Hoe ik deze opzet lees je ook in dit artikel.

## Voorvereisten

- Een **computer**, **server** of **VM** met **Mac OS X 10.6.8** en **xcode** geinstalleerd.
- Een **SSH** verbinding met die **computer**

## Perl 5.38.2 installeren vanaf source

Allereerst gaan we kijken welke versie van **Perl** er standaard op de **webdev omgeving** draait. Dit doe je met het volgende commando.

    perl --version
   
Bij mij is dat **5.10**, we gaan deze vervangen door **5.38.2**.

Ik heb deze versie al gedownload naar mijn **werk computer** en via **scp** kopieer ik deze naar de **webdev omgeving**.

	scp ~/Downloads/perl-5.38.2.tar.gz admin@webdev:~/Downloads 
		
Dan unzip ik deze met het volgende commando.

	tar -xzf perl-5.38.2.tar.gz
	
Als dit gedaan is ga ik in de map 'perl-5.38.2' in de 'Downloads' map.

Daar geef ik het volgende commando

	sudo ./configure -des -Dprefix=/usr/local
	
Let er op dat je altijd 'sudo' gebruikt in deze en 3 volgende stappen.

Als het vorige commando klaar is ga je de 'source' compileren met het commando

	sudo make
	
Dit kan wel even duren voordat dit commando klaar is, nadien volg je met volgend commando

	sudo make test
	
Dit duurt ook een tijdje.
Als laatste stap voer je volgend commando in

	sudo make install
	
Als dit klaar is, is de nieuwe versie van **Perl** geïnstalleerd.

## reroute Perl

Om de nieuwe versie standaard te gebruiken ga ik het **perl** commando vervangen door een link naar de nieuwe versie van **Perl**.

Als eerste het oude **perl** commando hernoemen, om later eventueel gemakkelijk te kunnen ongedaan maken.

	sudo mv /usr/bin/perl /usr/bin/perl-old
	
Nu ga je de symobolische link maken naar de nieuwe **Perl** versie. Bij mij staat die in '/usr/local/bin'. Dus volgende commando maakt deze symbolische link.

	ln -s /usr/local/bin/perl5.38.2 /usr/bin/perl
	
Nu kun je het commando geven om te kijken of de juste versie wordt gebruikt.

	perl --version
	
Bij mij geeft deze nu dus **5.38.2**

## installeer macports

**MacPorts** heb je nodig om **CURL** te installeren welke je nodig hebt om **Perl modules** te kunnen installeren.

Ik heb weer via **scp** de applicatie **macports** op de **webdev omgeving** gezet.

gebruik het 'installer' commando om **macports** vanaf de **commandline** te instaleren.

	sudo installer -pkg ~/Downloads/<macports file>.pkg -target /
	
Als dit gedaan is dan kun je **CURL** installeren met het volgende commando.

	sudo port install curl
	
Als je een error krijgt 'Port curl not found' doe dan een update van je systeem en herinstalleer je **macports**.

## CGI module installeren

NU ga ik de **CGI** **module** installeren en daarvoor heb ik cpanm.pl nodig, deze heb ik al gedownload en op de **webdev omgeving** gezet.

Met volgend commando installeer je **CGI** **module**

	sudo perl ./cpanm.pl CGI
	
Je moet dus in de map zijn waar **cpanm.pl** staat om dit te kunnen doen.

## Nu ga ik de setup van Apache doen

Eerst gaan we de **httpd.conf** file bekijken en aanpassen

	sudo vi /etc/apache2/httpd.conf
	
Kijk daar of volgende modules al geladen worden (bij mij is dit standaard het geval)

	LoadModule cgi_module libexec/apache2/mod_cgi.so
	LoadModule userdir_module libexec/apache2/mod_userdir.so
	
Als deze er al staan en geladen worden kun je het volgende toevoegen aan het **httpd.conf** bestand.

	Include /private/etc/apache2/extra/httpd-userdir.conf
	
Save en quit. Het volgende dat we gaan nakijken is de **httpd-userdir.conf** bestand.

	sudo vi /etc/apache2/extra/httpd-userdir.conf
	
Daar moet je kijken of onderstaande string aanwezig is (wat bij mij al het geval is)

	Include /private/etc/apache2/users/*.conf
	
Als dit niet het geval is, voeg je deze toe. Dan save en quit.

Nu gaan we de config file van de gebruiker aanpassen

	sudo vi /etc/apache2/usres/<short user name>.conf
	
Hier ga je het volgende toevoegen of aanpassen

	<Directory "/Users/<your short user name>/Sites/"> 
  		#AddLanguage en .en 
  		AddHandler cgi-script .cgi .pl .php
  		Options Indexes MultiViews FollowSymLinks ExecCGI 
  		AllowOverride None 
  		Order allow,deny
  		Allow fom all
  		Require host localhost
	</Directory>
	
Al je klaar bent dan weer save en quit.

Nu gaan we de permissies aanpassen van de **~/Sites** map zodat de **webserver** toegang heeft tot de **website**.

	chmod +a "_www allow execute" ~/Sites
	
nu gaan we de **Apache config file** testen op juistheid dit kan met het commando

	apachectl configtest
	
Als dit **OK** teruggeeft dan is de config file goed gemaakt.

We gaan nu de plist file laden met het commando

	sudo launchctl load -w /System/Library/LaunchDaemons/org.apache.httpd.plist
	
en dan de **Apache webserver** herstarten met

	sudo apachectl graceful
	
Nu kun je naar de website gaan met

	http://<ip address or name>/~<short user name>
	
## Aanmaken van test CGI page

We gaan nu in de **~/Sites** map een nieuwe map aanmaken waar alle **CGI** bestanden komen te staan, dit doe je met

	mkdir cgi-bin
	
Nu zou je de volgende map moeten hebben

	~/Sites/cgi-bin
	
Hier gaan we een test bestand maken met het volgende als inhoud

	#!/usr/bin/perl

	use CGI;

	my $cgi = CGI->new;

	print $cgi->header( -type => 'text/plain' );

	print "Hello From Perl!!";
	
save and quit

als laatste gaan we de net aangemaakte bestand **uitvoerbaar** maken met volgende commando

	chmod +x ~/Sites/cgi-bin/test.pl
	

Nu kun je testen door naar volgende **URL** te gaan in je **browser**

	http://<ip address>/~<short user name>/cgi-bin/test.pl
	
## Slotwoord

Nu heb je een werkende **Webdev** **ontwikkelomgeving** voor **HTML/CSS**, **vanilla javascript** en **Perl CGI** als **backend**.

Als je dit in een productie omgeving wilt laten werken kan het zijn dat je dingen moet aanpassen in de **httpd.conf** of **shortusername.conf** file. Bij mij is dat **admin.conf**.