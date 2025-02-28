# Abschlussbericht S3 Zeitbuchungssystem Marvin Volkmann

## Organisation
Die unterscheidlichen Rollen wurden am Anfag des Projektes eingeteilt, allerdings hielten wir sie meistens flexibel um uuns untereinander auszuhelfen. Organisiert haben wir uns mit Teams und github. In Teams konnten wir mithilfe der Planner App ein Kanban-Board erstellen, um dort dann in einer Aufgabenliste die unterschiedlichen Aufgaben einzuteilen. So hatten wir eine gute Übersicht und konnten immer wieder neue Aufgaben dazugeben und existierende Aufgaben abändern falls es notwendig war. Wir konnten einzelne Buckets erstellen die als Unterkategorien dienten. Ihnen fügten wir dann die einzelnen relevanten Aufgaben hinzu.

![Screenshot 2025-02-28 145215](https://github.com/user-attachments/assets/b6b01e86-1223-4337-8622-9de1f306b043)

## Code

Unseren Code haben wir in github geteilt. So konnte jeder seinen Teil hochladen und die anderen es einfach runterladen und bei sich selbst im Code einfügen.

![Screenshot 2025-02-28 150339](https://github.com/user-attachments/assets/a7c1e86c-4c49-40c5-a485-53d405bbcc3d)

Die Struktur war an der aus WinSCP angeordnet. So war alles klar und die Dateien lagen in den selben Ordnern wie dann auf dem Server. 

### Meetings
Meetings hatten wir meistens einmal die Woche um eventuelle FRagen oder Probleme zu klären und den weiteren Fortschritt zu besprechen. Diese Meetings wurden ebenfalls am Anfang des Projekts direkt organisiert. Allerdings haben wir auch vor allem gegen Ende des Projektes uns öffter getroffen, beziehungsweise Meetings an anderen Tagen gehalten.

## Eigenleistung

### Code

Mein Teil des Projekts war der Code um einen neuen Arbeitsbericht anzulegen. Um einen neuen Arbeitsbericht anzulegen entschieden wir uns dazu eine json zu erstellen in der die Arbeitsberichte gespeichert werden.
Die JSON erstellte ich im Ordner wo alle anderen JSON ebefalls lagen. 

Die graphische Erstellung des Arbeisberichtes war realtiv einfacher da ich dort nur die vorhergegebenen Punkte inklusive Eingabefelder angeben musste. Um die Startzeit und Endzeit aufzuzeichen benutzte ich folgendes:

                <div class="form-group">
                    <label for="startzeit">Startzeit</label>
                    <input type="datetime-local" id="startzeit" name="startzeit" required>
                </div>
                <div class="form-group">
                    <label for="endzeit">Endzeit</label>
                    <input type="datetime-local" id="endzeit" name="endzeit" required>
                </div>
So kann man zwar selbst das Datum und die Uhrzeit eingeben, aber auchb einfach mithilfe der Kalenders der auftaucht wenn man auf das Kalender zeichen draufdrückt.

Die Eingabe im Pausenfeld habe ich so geregelt, das man nur eine Nummer eingeben kann, die nur Positiv sein kann und bei 0 startet. Das Feld wurde als breaktime definiert

                <input type="number" id="pause" name="breaktime" min="0" step="1" value="0">

Danach kommen die Felder Modul auswählen, Berichtsname und Kommentare. Bewrichtsname und Kommentare sind einfache Eingabefelder. Mit select erstellte ich ein Dropdown-Menü wo man die existierenden Module asuswählen kann. 
Mit folgendem konnte ich die Liste der Module mit dem Auswahlfeld verknüpfen.

               {% for key, value in module.items %}

Nachdem ich die Html und CSS Dateien erstelle konnte ich mich auf die views.py konzentrieren.

Als erstes muss das Modul dictionary geladen werden. 

               try:
                 with open(datenbank_module, "r", encoding="utf-8") as file:
                 module = json.load(file)["module"]
               except (FileNotFoundError, json.JSONDecodeError):
                 return HttpResponseBadRequest("Fehler beim Laden der Module-Datei.")

datenbank_module greift auf die module.json zu. Dies habe ich bei den Pfaden zu den verschiedenen JSON Dateien geregelt

                datenbank_module = "/var/www/buchungssystem/db/module.json"

Nun stellte sich uns die Frage wie wir die Arbeitsberichte untereinander unterschieden, da alle in die selbe JSOn kommen. Nach einer kleinen Recherche entschieden wir uns für UUID. Diese mussten wir mit import uuid importieren um sie benutzen zu können. So gab ich jedem Arbeitsbericht eine neue uuid. 

Nun muss ein neuer Arbeitsbericht erstellt werden. Zuerst wird ein neues dictionary erstellt mit den Information. Dann wird dieses dictionary in der Arbeitsberichte.json gespeichert

![Screenshot 2025-02-28 165906](https://github.com/user-attachments/assets/383efd93-5d3e-48b3-9fe6-90489909f89e)

Danach fehlte nurnoch die Berechnung der Zeit. 

![Screenshot 2025-02-28 170227](https://github.com/user-attachments/assets/b4d29062-c3db-4413-8c37-8c54e90d0729)

### Testen

Des weiteren hilf ich viel Dabei den Code der anderen zu testen. So haben Sie ihren Code in github hochgeladen, och übernahm ihn und prüfte ihn ob irgendwie Fehler auftraten. Ein Beispiel das gegen Ende aufkam war, wenn man einen neuen Benutzer erstellt die Informationen in der Arbeitsberichte.json mitgesperichert wurden. Dadurch hatte man dann keinen Zugriff mehr auf die Profil Seite. Diesen Fehler entdeckte ich und konnte ihn nach absprache mit den anderen eliminieren. 
