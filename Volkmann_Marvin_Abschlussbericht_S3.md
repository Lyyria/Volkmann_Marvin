# Abschlussbericht S3 Zeitbuchungssystem Marvin Volkmann

## Organisation
Die unterscheidlichen Rollen wurden am Anfag des Projektes eingeteilt, allerdings hielten wir sie meistens flexibel um uns untereinander auszuhelfen. Organisiert haben wir uns mit Teams und github. In Teams konnten wir mithilfe der Planner App ein Kanban-Board erstellen, um dort dann in einer Aufgabenliste die unterschiedlichen Aufgaben einzuteilen. So hatten wir eine gute Übersicht und konnten immer wieder neue Aufgaben dazugeben und existierende Aufgaben abändern falls es notwendig war. Wir konnten einzelne Buckets erstellen die als Unterkategorien dienten. Ihnen fügten wir dann die einzelnen relevanten Aufgaben hinzu.

![Screenshot 1](https://i.imgur.com/rDLzzEE.png)

## Code

Unseren Code haben wir in github geteilt. So konnte jeder seinen Teil hochladen und die anderen es einfach herunterladen und bei sich selbst im Code einfügen.

![Screenshot 2](https://i.imgur.com/3vvyZlB.png)

Die Struktur war an der aus WinSCP angeordnet. So war alles klar und die Dateien lagen in den selben Ordnern wie dann auf dem Server. 

### Meetings
Meetings hatten wir meistens einmal die Woche, um eventuelle Fragen oder Probleme zu klären und den weiteren Fortschritt zu besprechen. Diese Meetings wurden ebenfalls am Anfang des Projekts direkt organisiert. Allerdings haben wir auch vor allem gegen Ende des Projektes uns öfter getroffen, beziehungsweise Meetings an anderen Tagen gehalten.

## Eigenleistung

### Code

Mein Teil des Projekts war der Code um einen neuen Arbeitsbericht anzulegen. Um einen neuen Arbeitsbericht anzulegen entschieden wir uns dazu eine JSON zu erstellen in der die Arbeitsberichte gespeichert werden.
Die JSON erstellte ich im Ordner wo alle anderen JSON ebefalls lagen. 

Die graphische Erstellung des Arbeisberichtes war realtiv einfacher da ich dort nur die vorgegebenen Punkte inklusive Eingabefelder angeben musste. Um die Startzeit und Endzeit aufzuzeichen benutzte ich folgendes:

                <div class="form-group">
                    <label for="startzeit">Startzeit</label>
                    <input type="datetime-local" id="startzeit" name="startzeit" required>
                </div>
                <div class="form-group">
                    <label for="endzeit">Endzeit</label>
                    <input type="datetime-local" id="endzeit" name="endzeit" required>
                </div>
So kann man zwar selbst das Datum und die Uhrzeit eingeben, aber auch einfach mithilfe des Kalenders, der auftaucht wenn man auf das Kalender zeichen drückt.

Die Eingabe im Pausenfeld habe ich so geregelt, das man nur eine Nummer eingeben kann, die nur Positiv sein kann und bei 0 startet. Das Feld wurde als breaktime definiert.

                <input type="number" id="pause" name="breaktime" min="0" step="1" value="0">

Danach kommen die Felder Modul auswählen, Berichtsname und Kommentare. Berichtsname und Kommentare sind einfache Eingabefelder. Mit select erstellte ich ein Dropdown-Menü wo man die existierenden Module asuswählen kann. 
Mit folgendem konnte ich die Liste der Module mit dem Auswahlfeld verknüpfen.

               {% for key, value in module.items %}

Nachdem ich die Html und CSS Dateien erstellte konnte ich mich auf die views.py konzentrieren.

Als erstes musste das Modul dictionary geladen werden. 

               try:
                 with open(datenbank_module, "r", encoding="utf-8") as file:
                 module = json.load(file)["module"]
               except (FileNotFoundError, json.JSONDecodeError):
                 return HttpResponseBadRequest("Fehler beim Laden der Module-Datei.")

datenbank_module greift auf die module.json zu. Dies habe ich bei den Pfaden zu den verschiedenen JSON Dateien geregelt

                datenbank_module = "/var/www/buchungssystem/db/module.json"

Nun stellte sich uns die Frage wie wir die Arbeitsberichte untereinander unterschieden, da alle in die selbe JSON kommen. Nach einer kleinen Recherche entschieden wir uns für UUID. Diese mussten wir mit import uuid importieren um sie benutzen zu können. So gab ich jedem Arbeitsbericht eine neue uuid. 

Nun musste als erstes ein neuer Arbeitsbericht erstellt werden. Zuerst wird ein neues dictionary erstellt mit den Information. Dann wird dieses dictionary in der Arbeitsberichte.json gespeichert

![Screenshot 3](https://i.imgur.com/A14cKkg.png)

Danach fehlte nurnoch die Berechnung der Zeit. 

![Screenshot 4](https://i.imgur.com/WhDyJLN.png)

Als letztes fügte ich noch die Sperre hinzu, das man nur auf die Seite zugreifen kann, wenn man angemeldet ist. Diee benutzen wird öfters.

### Testen

Des weiteren hilf ich viel dabei, den Code der anderen zu testen. So haben Sie ihren Code in github hochgeladen, ich übernahm ihn und prüfte ihn ob irgendwelche Fehler auftraten. Ein Beispiel das gegen Ende aufkam war, das wenn man einen neuen Benutzer erstellt, die Informationen in der Arbeitsberichte.json mitgespeichert wurden. Dadurch hatte man dann keinen Zugriff mehr auf die Profil Seite. Diesen Fehler entdeckte ich und konnte ihn nach absprache mit den anderen eliminieren. 
