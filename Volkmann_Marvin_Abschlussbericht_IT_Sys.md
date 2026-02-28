# Abschlussbericht IT Sys Staat neu denken Marvin Volkmann

## Organisation
Das Projekt startete indem wir als erstes unseren Bereich aussuchten. Wir entschieden uns für das Meldeamt, wa wir eine eher generelle Rolle einnehmen wollten und wir so einige Grund datenbanken erstellen und verwalten konnten, auf die die anderen Teams zugreifen konnten. Auch diesesmal entschieden wir uns für eine Teams Gruppe um unseren Progress, Meetinmgs und Daten zu verwalten. Mit dem eingebauten Kanban-Board waren wir bereits vertraut und konnten es so leicht benutzen. Damit aber die gesamte Gruppe Daten austauschen konnten wurde noch extra auf notion eine Gruppe eingerichtet. Hier hatte jeder seine Spaces wo man Sachen hochladen konnte und der Rest dann drauf zugreifen konnte. Damit wir einen guten Plan für unsere Aufgaben hatten erstellten wir einen Projektplan und teilten uns das Semester in 3 Sprints auf. So hatten wir alle Aufgaben in einer Übersicht mit Deadlines. Die ersten paar Wochen fokussierten wir uns fast ausschließlich auf Wissenssammlung. So konnten wir herausfinden welche Dienste wir anbieten wollen und welche Datenbanken sowohl von uns, aber auch von den anderen gebraucht werden. Uns wurde aber schnell klar dass das Kanban Board von Teams zu schwach und ungenau war um uns eine gute Übersicht über alle Aufgaben zu bieten. Daher wechselten wir nach kurzer Zeit zu jira, das in den Bereichen deutlich besser war. 

![Screenshot_1](https://i.imgur.com/pYXeAig.png)

Durch die Einteilung in Sprints konnten wir die Aufgaben gut einteilen. Wir wollten in Sprint 1 hauptsächlich Datenbanken und das Frontend priorisieren. In Sprint 2 dann das Backend mit REST-API's und Sprint 3 eigentlich nur noch die letzten Fehler fixen und alles noch schöner machen.

### Code

Den Code teilten wir wieder in Github, allerdings haben wir diesesmal unsere GitHub mit VS Code verbunden. So konnten wir durch Pull und Push die Dateien in VS Code von GHitHub pullen, berarbeiten und dann wieder auf GitHub pushen. jeder konnte so dann den Code bearbeiten und musste nur pullen um die aktuelle Version zu haben. 

![Screenshit_2](https://i.imgur.com/PeSaKXg.png)

In den Ordnersn lagen dann die Templates, .views, .urls, und Datenbanken.

### Meetings

Meetings wurden einmal in der Woche gemacht. Dabei wurden dann meistens Fragen geklärt, Ergebnisse besprochen und neue Aufgaben erstellt. Danach wurden meistens der Termin des nächsten Meetings besprochen. Vorallem gegen Ende des Projekts hatten wir dann auch teilweise zwei Meetings in der Woche um die letzten Probleme zu klären. In dr gesamten Gruppe wurde dann auch ein Führungskreis bestimmt, der sich ebenfalls einmal die Woche traf. Hier wählten wir eine Person aus die immer teilnahm.

## Eigenleistung

### Organisation

Bei mir war es der Plan, dass ich in Sprint 1 die Datenbanken Wohnsitzregister und Adressenregister erstelle, in Sprint 2 das Formular Wohnsitz ändern und in Sprint 3 noch Fehler ausbessere.

### Code

Mein Code Teil des Projekts war es zuerst die Datenbanken für das Wohnsitzregister und das Adressenregister zu erstellen.

    [
    {
        "meldungsvorgang_id": "ccccd023-5fc8-489e-867a-d4a8663c6773",
        "adresse_id": "0d3942b6-87d7-4bad-9b5a-5257e466554a",
        "buerger_id": "e11111111-2222-3333-4444-555555555555",
        "straße_hausnummer": "Musterweg_1",
        "plz_ort": "123456_Musterdorf",
        "land": "DE",
        "anmmeldung_datum": "2025-01-21",
        "abmeldung_datum": "2025-02-21",
        "wohnsitz_aktuell": true  
    }
    ]

Da jeder Bürger sein wohnsitz anmelden muss wird, sobald er das getan hat, im Wohnsitzregister eine ID über seine Wohnsitz-Anmeldung angelegt. Gleichzeitig steht auch eine Adressen-ID drin, wo er genau wohnt. 

Für das Adressenregister nahm ich einige zufällige Adressen aus Stuttgart und fügte diese durch adresse_id, straße_hausnummer, plz_ort und land in das register hinzu. 

    {
    "adressenregister": [
        {
            "adresse_id": "d3942b6-87d7-4bad-9b5a-5257e466554a",
            "straße_hausnummer": "Marktplatz 1",
            "plz_ort": "70173 Stuttgart",
            "land": "The Länd"
        },
        {
            "adresse_id": "d3942b6-87d7-4bad-9b5a-5257e466554b",
            "straße_hausnummer": "Hahnemannstraße 1",
            "plz_ort": "70191 Stuttgart",
            "land": "The Länd"
        },
        {
            "adresse_id": "d3942b6-87d7-4bad-9b5a-5257e466554c",
            "straße_hausnummer": "Königstraße 1A",
            "plz_ort": "70173 Stuttgart",
            "land": "The Länd"

Mit diesem Register konnten wir dann unteranderem bei unserem Formular "Wohnsitz ändern" die Adressen als Auswahl angeben. Ausßerdem erstellten wir eine Karte wo man die verschiedenen Wohnsitze sehen kann.

![Screenshot_3](https://i.imgur.com/YHCgCSC.png)

Eine weitere Aufgabe war es dann die Pfade zu den Registern korrekt in die views einzufügen und Hilfs funktion zum aufrufen und persistieren von Daten in den Registern.

    def lade_adressenregister():                                                          
    try:
        with open(adressenregister, "r", encoding="utf-8") as datei:
            return json.load(datei)
    except:
        return {"adressenregister": []}

    def lade_wohnsitzregister():                                                           
    try:
        with open(wohnsitzregister, "r", encoding="utf-8") as datei:
            return json.load(datei)
    except:
        return []   
    
    def speichere_wohnsitzregister(daten):                                               
    with open (wohnsitzregister, "w", encoding="utf-8") as datei:
        json.dump(daten, datei, ensure_ascii=False, indent=2)

Der großteil meiner Arbeit in Sprint 2 war es dann das Formular Wohnsitz anmelden zu erstellen. Hierbei ersellte ich zuerst eine Funktion die überprüft ob der Benutzer angemeldet ist. Danach werden die Adressen aus dem Adressenregister geladen und dem Formular zur Verfügung gestellt.Wenn dann ein POST-Request kommt wird der ausgewählte Vorgang sowie die Benutzerrolle aus der Session ausgelesen.

    def buerger_services(request):

    if not request.session.get("user_id"):
        return redirect("login")

    daten_adressen = lade_adressenregister()
    liste_adressen = daten_adressen.get("adressenregister", [])

    adressen = []
    for neue_adresse in liste_adressen:
        label = f'{neue_adresse["straße_hausnummer"]}, {neue_adresse["plz_ort"]}'
        adressen.append({"id": neue_adresse["adresse_id"], "label": label})

    if request.method != "POST":
        error = request.session.pop("error", None)
        return render(request, "einwohnermeldeamt/buerger_services.html", {
            "adressen": adressen,
            "error": error
        })

    vorgang = request.POST.get("Formulare_Meldeamt")
    role = request.session.get("role", "buerger")

Nun ersellte ich eine Funktion, die überprüft, ob der Benutzer angemeldet ist, da nur eingeloggte Nutzer auf diesen Bereich zugreifen dürfen. Anschließend werden die Adressen aus dem Adressenregister geladen und dem Formular zur Auswahl zur Verfügung gestellt. Wenn ein POST-Request eingeht, wird der ausgewählte Vorgang sowie die Benutzerrolle aus der Session ausgelesen. Danach muss ich die übermittelten Daten prüfen, gleiche sie mit den Registern ab und speichere bei erfolgreicher Validierung den neuen Wohnsitz im entsprechenden Register.

       if vorgang == "wohnsitz":

        if role != "buerger":
            return render(request, "einwohnermeldeamt/buerger_services.html", {
                "adressen": adressen,
                "error": "Nur Bürger dürfen Wohnsitz anmelden."
            })

        adresse_id = request.POST.get("adresse_id")
        buerger_id = request.POST.get("buerger_id")

        bestehende_adresse = None
        for neue_adresse in liste_adressen:
            if neue_adresse["adresse_id"] == adresse_id:
                bestehende_adresse = neue_adresse
                break

        daten_personen = lade_personenstandsregister()
        person = None
        for eintrag in daten_personen:
            if eintrag.get("buerger_id") == buerger_id:
                person = eintrag
                break

        if not bestehende_adresse or not person:
            return render(request, "einwohnermeldeamt/buerger_services.html", {
                "adressen": adressen,
                "error": "Bürger-ID oder Adresse nicht gefunden."
            })

        neuer_eintrag = {
            "meldungsvorgang_id": str(uuid.uuid4()),
            "adresse_id": adresse_id,
            "buerger_id": buerger_id,
            "straße_hausnummer": bestehende_adresse["straße_hausnummer"],
            "plz_ort": bestehende_adresse["plz_ort"],
            "land": bestehende_adresse["land"],
        }

        person["adresse"] = adresse_id
        speichere_personenstandsregister(daten_personen)

        wohnsitz_daten = lade_wohnsitzregister()
        wohnsitz_daten.append(neuer_eintrag)
        speichere_wohnsitzregister(wohnsitz_daten)

        datum_heute = date.today().strftime("%d.%m.%Y")

Da wir auch eine PDF erstellen wollten arbeitete ich mit Stefan zusammen, sodass man das Formular ausfüllt und dann eine PDF mit den Daten bekommt. Wir entschieden uns hier mit FPDF zu arbeiten. Damit die PDF auch schön aussieht erstellten wir ein Logo. Die PDF besteht dann am Ende aus den persönlichen Daten der Person, das aktuelle Datum sowie die vollständige Adresse formatiert. Außerdem fügten wir noch Daten wie die Bürger-ID und die Meldevorgang-ID hinzu. Anbschließend speichern wir die PDF-Datei im System, erstellen eine HTTP-Response mit dem Content-Type "application/pdf" damit die PDF direkt im Browser ausgegeben wird und heruntergeladen werden kann.

     pdf = FPDF()
        pdf_base(pdf, "Meldebestätigung", logo_path=logo_path)

        pdf.set_font("Helvetica", "", 12)
        pdf.cell(0, 8, "Hiermit wird bestätigt, dass", ln=True)
        pdf.ln(2)

        pdf.set_font("Helvetica", "B", 13)
        pdf.cell(0, 8, f"{person.get('vorname','')} {person.get('nachname_geburt','')}", ln=True)
        pdf.ln(2)

        pdf.set_font("Helvetica", "", 12)
        pdf.multi_cell(0, 8, f"am {datum_heute} seinen Wohnsitz an folgender Adresse angemeldet hat:")
        pdf.ln(4)

        pdf.set_x(25)
        pdf.multi_cell(
            0, 8,
            f"{bestehende_adresse['straße_hausnummer'].replace('_',' ')}\n"
            f"{bestehende_adresse['plz_ort'].replace('_',' ')}\n"
            f"{bestehende_adresse['land']}"
        )
        pdf.set_x(10)
        pdf.ln(6)

        pdf_meta_block(pdf, [
            f"Bürger-ID: {buerger_id}",
            f"Meldungsvorgang-ID: {neuer_eintrag['meldungsvorgang_id']}",
        ])

        pdf_meldebestaetigung = pdf.output(dest="S").encode("latin-1")
        dateiname = f"meldebestaetigung_{date.today().isoformat()}_{neuer_eintrag['meldungsvorgang_id']}.pdf"
        dokument_speichern(buerger_id, "meldebestaetigung", pdf_meldebestaetigung, dateiname)
        response = HttpResponse(pdf_meldebestaetigung, content_type="application/pdf")
        response["Content-Disposition"] = 'inline; filename="meldebestaetigung.pdf"'
        return response

        
### Theorie

Bei der erstellung der Formulare orinteirten wir uns an der realen Welt, wollten die Prozesse aber schneller und einfacher gestalten. So sollte der Benutzer möglichst unkkomplizierte und übersichtliche Schritte ausführen um sein Ziel zu erreichen. Tatsöchlich gibt es inziwschen einen ähnlichen Vorgang auf der offiziellen Website der Bundesrepublik Deutschland. Hierbei kann man anscheinend online und ohne Behördengang, also so wie bei unserem Staat, einen neuen Wohnsitz anmelden und seine Ausweis, Reisepass oder eID-Karte aktuialisieren.

![Screenshot_4](https://i.imgur.com/ZbrOjzx.png)

## Fazit

Auch dies war wieder ein sehr interessantes Projekt. Obwohl die Grenzen am Anfang noch relativ ungenau waren, kam am Ende ein sehr gutes Projekt heraus. Nun weiß man wie die Prozesse am besten ablaufen sollten in den Kommunen und auch wie das auf der technischen Seite aussieht.







