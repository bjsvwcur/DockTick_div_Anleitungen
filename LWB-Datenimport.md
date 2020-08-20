# Nachführung LWB-Datenimport (MGDM-Lieferung) an die Aggregationsinfrastruktur
Verantwortlicher: bjsvwcur

## Beschreibung:
LWB-Datenlieferung in Aggregation Infrastruktur importieren

## Auftrag:
"H:\BJSVW\Agi\Projekte\ALW\MGDM-AI-Lieferungen ID151 ID153"

## Ablauf Nachführung:
###	Vorbereitung der Daten
Die Daten müssen entsprechend den Modellen gezippt werden.

* Rebbaukataster

  `SO_a_151_1_rebbaukataster_V1_2_30171130.xtf`  zu `SO_a_151_1.xtf` umbenennen und als `SO_151_1.zip` zippen.

* Nutzungsflächen

  `SO_a_153_6_bewirtschaftungseinheiten_V1_3_20171201.xtf` zu `SO_a_153_6.xtf` umbenennen und `SO_b_153_1_nutzungsflaechen_20171201.xtf` zu `SO_b_153_1.xtf` umbenennen und in einer Zip-Datei (`SO_153_1.zip`) zippen.

* Biodiversitätsförderflächen Qualitätsstufe II und Vernetzung

  `SO_c_153_3_biodiversitaetsfoerderflaechen_qualitaetsstufe_II_und_vernetzung_V1_4_20171201.xtf` zu `SO_c_153_3.xtf` umbenennen und zusammen mit `SO_a_153_6.xtf` und `SO_b_153_1.xtf` als `SO_153_3.zip` zippen.

* Perimeter LN- und Sömmerungsflächen

  `SO_a_153_5_perimeter_ln_soemmerungsflaechen_V1_3_20171201_SO.xtf` zu `SO_a_153_5.xtf` umbenennen und als `SO_153_5.zip` zippen.

* Bewirtschaftungseinheit

  `SO_a_153_6_bewirtschaftungseinheiten_V1_3_20171201.xtf` zu `SO_a_153_6.xtf` und als `SO_153_6.zip` zippen.

### Import der Daten in die Integrationsumgebung
Auf der Webseite `https://integration.geodienste.ch` einloggen. Benutzername1 und PW1 siehe KeepPass. 
Im Anschluss muss man sich auf `https://integration.geodienste.ch` unter "Login" erneut einloggen. Benutzername2 und PW2 siehe KeePass.

* Topic der Angebote ermitteln (definitive Daten)
Wechsel auf die Seite Datenintegration > Konfiguration. Hier kann für jedes Angebot die technische Bezeichnung (topic) nachgeschlagen werden:
  * Rebbaukataster
    - Angebot: Landw. Bewirtschaftung: Rebbaukataster 
    - Topic: lwb_rebbaukataster
  * Nutzungsflächen
    - Angebot: Landw. Bewirtschaftung: Nutzungsflächen
    - Topic: lwb_nutzungsflaechen
  * Biodiversitätsförderflächen Qualitätsstufe II und Vernetzung
    - Angebot: Landw. Bewirtschaftung: Biodiversitätsförderflächen, Qualitätsstufe II und Vernetzung
    - Topic: lwb_biodiversitaetsfoerderflaechen
  * Perimeter LN- und Sömmerungsflächen
    - Angebot: Landw. Bewirtschaftung: Perimeter LN- und Sömmerungsflächen
    - Topic: lwb_perimeter_ln_sf
  * Bewirtschaftungseinheit
    - Angebot: Landw. Bewirtschaftung: Bewirtschaftungseinheit
    - Topic: lwb_bewirtschaftungseinheit
* Topic der Angebote ermitteln (provisorische Daten)
Wechsel auf die Seite Datenintegration > Konfiguration. Hier kann für jedes Angebot die technische Bezeichnung (topic) nachgeschlagen werden:
  * Perimeter LN- und Sömmerungsflächen
    - Angebot: Landw. Bewirtschaftung: Perimeter LN- und Sömmerungsflächen, verwaltungsintern
    - Topic: lwb_perimeter_ln_sf_intern
  * Bewirtschaftungseinheit
    - Angebot: Landw. Bewirtschaftung: Bewirtschaftungseinheit, verwaltungsintern
    - Topic: lwb_bewirtschaftungseinheit_intern
* Datenupload
Zip-Files einzeln mittels curl in die REST-Schnittstelle hochladen.
```
z.B.
curl -u <BENUTZERNAME2>:<PASSWORD2> -F topic= lwb_perimeter_ln_sf -F lv95_file=@/home/barpastu/shares/sogis_home/2018_MGDM_ALW/SO/SO_153_5.zip –F publish=true "https://integration.geodienste.ch/data_agg/interlis/import"
```
* Datenimport
Wechsel auf die Seite Datenintegration > Datenimport. Grundsätzlich werden Files, welche hochgeladen wurden automatisch hochgeladen. Falls nicht, kann der Datenimport manuell gestartet werden. 
Auf dieser Seite kann der Stand der Datenimporte geprüft werden.

* Datenveröffentlichung
Nach dem die Daten hochgeladen und importiert wurden müssen sie auch noch veröffentlich werden. Dazu muss auf die Seite Datenintegration > Veröffentlichung gewechselt werden. Bei Angebot wird das entsprechend zu veröffentlichende Angebot ausgewählt und bei Geplanter Start der Startzeitpunkt für die Veröffentlichung angegeben.

### Import der Daten in die Produktionsumgebung
Auf die Webseite `https://geodienste.ch` mit Benutzer (<BENUTZERNAME2> / <PASSWORD2>) einloggen und anschliessend gleiches Vorgehen wie bei Kapitel 2.
```
URL in curl-Aufruf ändern. Z.B.
curl -u <BENUTZERNAME2>:<PASSWORD2> -F topic= lwb_perimeter_ln_sf -F lv95_file=@/home/barpastu/shares/sogis_home/2018_MGDM_ALW/SO/SO_153_5.zip –F publish=true "https://geodienste.ch/data_agg/interlis/import"
```



