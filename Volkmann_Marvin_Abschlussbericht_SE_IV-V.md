# Abschlussbericht SE IV-V Desk-Sharig Tool Marvin Volkmann

## Organisation
Bereits vor dem Start des Projekts fingen wir an , uns mit dem Raspberry Pi ausanderzusetzen. Wir erstellten unsere Teams Gruppe. Da wir im vorherigen Projekt bereits mit dem eingebauten Kanban-Board gearbeitet haben, viel es uns leicht es diesesmal wieder zutun. Um unseren Code gegeinander auszutauschen, benutzten wir wiedr Github. Über den gesamten Februar machten wir uns also daran Wissen aufzubauen, damit wir bereits eine solide Wissensbasis zum Start des Projekts hatten. So wussten wir bereit auch früh was unser Projekt werden sollte. Am Start des Projekts haben wir uns dann in die unterschiedlichen Rollen eingeteilt, wobei wir es recht flexibel hielten um uns gengeseitig unterstützen zu können. Im Kanban-Boar dtrugen wir die einzlnen Teilaufgaben ein, beziehungsweise teilten alles grob auf in Frontend, Backend und GPIO. So hatte jeder eine gute Übersicht über eine einzelnen Aufgaben. Weitere Aufgaben konnten wir dazugeben und existierende abändern, falls es nötig war. Weitere Kategorien waren Wissens-Management, Planung/Vorbereitung, Einkauf Material und IT-Messe. Somit hatten wir eine klare Übersicht über alle möglichen Aufgaben.

![Screenshot 1](https://i.imgur.com/B1AZpYO.png)

Durch die Vorgabe der Sprints konnten wir die Aufgaben gut einteilen. Wir wollten im Sprint 1 hauptsächlich das Frontend und Materialien priorisieren. In Sprint 2 dann das Backend und GPIO Funktionen und in Sprint 3 dann das Modell und Probleme beheben. 

### Code

Den Code teilten wir wie bereits erwähnt auf Github. Es wurde zuerst jede Datei des Servers hochgeladen. Danach konnte dann jeder die Datei an der er arbeitete einfach überschreiben und die anderen konnten es sich so auf ihren eigenen Server ziehen. 

![Screenshot 2](https://i.imgur.com/KfvGl6L.png)

In den Ordnern lagen dann die Templates, .views, .urls, Datenbanken sowie die GPIO Funktionen.

### Meetings

Meetings wurden einmal in der Woche gemacht. Dabei wurden dann meistens Fragen geklärt, Ergebnisse besprochen und neue Aufgaben erstellt. Danach wurden meistens der Termin daes nächsten Meetings im aktuellen besprochen. Vorallem gegen Ende des Projekts hatten wir dann auch teilweise zwei Meetings in der Woche um die letzten Probleme zu klären.

## Eigenleistung

### Organisation

Bei mir war es der Plan, dass ich in Sprint 1 Materialien beschaffe und die GPIO Funktionen für die LEDs schreibe. In Sprint 2 erstellte ich dann die Tageslichtsensorfunktionen und in Sprint 3 erstellte ich das Modell.

### Vorbereitung

In der Vorbereitung war mein Teil die nötigen Sensoren, Kabel und ähnliches zu finden. Da wir bereits letztes Semester an einem Zeitbuchungssystem arbeiteten und ein Desk-Sharing-Tool eine Erweiterung davon ist, wussten wir ziemlich genau was wir einbauen wollten. Ich musste also nach einem möglichen Tageslichtsensor suchen. Außerdem gewisse LEDs die Rot und Grün leuchten können um damit die Belegung des Arbeitsplatz zu zeigen. Des Weiteren musste ich als Vorbereitung der Verkabel Skizzen erstellen, wie die LEDs und der Tageslichtsensor verkabelt werden können. 
Dies machte ich mithilfe dem Programm Fritzing. Ich erstellte dadurch verschiedene mögliche Skizzen, womit alle dann wussten, wie sie die Kabel einstecken mussten.

![Screenshot 3](https://i.imgur.com/5lTsIkK.png)

### Code

Mein Code Teil des Projekts war es die GPIO Funktionen zu erstellen. Also damit die LEDs grün leuchten wenn ein Arbeitsplatz offen ist und rot leuchten wenn er belegt ist. Dafür erstellte ich die led_control.py Datei, die dann in den Projekt Ordner hineinkam. Bei der Erstellung der Funktionen orientierte ich mich an dem Skript Unit 2 Relais, da dort das GPIO.setup bereits grob vorgegeben wurde.

              GPIO.setmode(GPIO.BCM)
              GPIO.setwarnings(False)

              def setup_pins(red_pin, green_pin):
                  GPIO.setup(red_pin, GPIO.OUT)
                  GPIO.setup(green_pin, GPIO.OUT)


              def led_off(red_pin, green_pin):
                  GPIO.output(red_pin, GPIO.LOW)
                  GPIO.output(green_pin, GPIO.LOW)

              def  led_red(red_pin, green_pin):
                  GPIO.output(red_pin, GPIO.HIGH)
                  GPIO.output(green_pin, GPIO.LOW)

              def led_green(red_pin, green_pin):
                  GPIO.output(red_pin, GPIO.LOW)
                  GPIO.output(green_pin, GPIO.HIGH)

Hierbei setzte ich zuerst den Modus auf den Broadcom SOC channel. Dadurch bezogen wir uns auf die internen Nummern der Pins. Danach musste ich zwei GPIO Pins als Ausgänge konfigurieren. Jeweils einen für die zei verschiedenen Farben.

Danach musste ich die Funktionen schreiben, wann die LED aus ist, auf Rot geht und wann auf Grün geht. Um sie auszuschalten muss natürlich keine Spannung da sein, daher ist es zweimal GPIO.LOW
Damit Die LED rot leuchtet, muss der rote Pin GPIO.High sein und der grüne Pin GPIO.LOW sein damit der Strom fließt.
Wenn sie grün leuchten soll, muss es einfach das gegenteil von Rot sein. 

Eine weitere Funktion brauchte es dann noch, um die Datei in der views.py zu verwenden.

              def set_led_status(gpio_red, gpio_green, status):
                  setup_pins(gpio_red, gpio_green)

                  if status == "frei":
                      led_green(gpio_red, gpio_green)
                  elif status == "belegt":
                      led_red(gpio_red, gpio_green)
                  else:
                      led_off(gpio_red, gpio_green)

Hierbei musste ich es so machen, dass die LED dann auch tatsächlich die Farben aktiviert. Wenn der Status frei ist soll sie grün leuchten, wenn er belegt ist rot und wenn gar nichts ist, soll sie aussein.

Nun musste ich diese Datei auch mit der views.py verbinden. Dafür brauchte ich die set_led_status Funktion.

              from .led_control import set_led_status

So konnte ich sie mir holen. In der views.py fügte ich dann den folgenden Code hinzu.

              if "gpio_red" in arbeitsplatz and "gpio_green" in arbeitsplatz:
                  try:
                      set_led_status(
                          arbeitsplatz["gpio_red"],
                          arbeitsplatz["gpio_green"],
                          arbeitsplatz["status"]

Hier griff ich auch die arbeitsplaetze.json zu, in der wir die Arbeitsplätze vordefinierten und ihnen jeweils einen gpio_red und gpio_green Pin zuwiesen. Danach musste dann die set_led_status miteingebunden werden. Die verschiedenen Statuse wurden bereits in der led_control.py definiert.

![Screenshot 4](https://i.imgur.com/m3QQ9Mp.png)

Eine weitere Aufgabe meinerseits war es den Tageslichtsensor miteinzubinden. Auch dafür erstellte ich eine Datei im Projekt Ordner die ich tageslichtsensor.py nannte. Um den Sensor zu verwenden musste ich einige Module importieren. smbus2 um mit dem Sensor zu kommunizieren. time damit er nur alle 10 Seekunden misst und threading damit jeder sensor einen eigenen Mess-Thread hat.
Des Weiteren erstellte ich 3 Dictionarys um die Sensor threads zu verwalten, Stop signale zu haben und um die Luxwerte zwischenzuspeichern.

            import smbus2
            import time
            import threading

            sensor_threads = {}
            stop_events = {}
            luxwerte = {}

Danch erstellte ich vier Funktionen. Eine für die dauerhafte Messung, eine um den Sensor zu starten und eine um ihn zu stoppen und eine um die Luxwerte zu bekommen.

            def dauerhafte_messung(arbeitsplatz_id, i2c_bus, i2c_address):
                bus = smbus2.SMBus(i2c_bus)
                mode = 0x10
                stop_event = stop_events[arbeitsplatz_id]

                while not stop_event.is_set():
                  try:
                      bus.write_byte(i2c_address, mode)
                      time.sleep(0.15)
                      data = bus.read_i2c_block_data(i2c_address, 0x00, 2)
                      lux = ((data[0] << 8) + data[1]) / 1.2
                      luxwerte[arbeitsplatz_id] = round(lux, 2)
                      print(f"[{arbeitsplatz_id}] {lux:.2f} Lux")
                  except Exception as e:
                      print(f"[{arbeitsplatz_id}] Sensorfehler:", e)
                      luxwerte[arbeitsplatz_id] = -1
                  time.sleep(5)

            bus.close()

Hier musste ich zuerst den I²C Bus öffnen, den richtigen Modus einstellen und ein stop event einbauen um das Stoppen zu steuern. Der Hauptteil besteht aus dem auslesen der Sensorergebnisse und dem dem daraus berechnenden Luxwert. Der Luxwert wir dann im dafür erstellten Dictionary gesperichert und ausgegeben. time.sleep(0.15) ist dafür, da der Sensor Zeit benötigt um zu messen.

Dann erstellte ich nich eine Exception, falls Fehler auftreten sollten. Vor der nächsten Messung sollter er dann 5 Sekunden warten.

die Funktion start_sensor ist dafür um einen neuen Thread zu starten, falls noch keiner läuft.

            def start_sensor(arbeitsplatz_id, i2c_bus=1, i2c_address=0x23):
                if arbeitsplatz_id in sensor_threads and sensor_threads[arbeitsplatz_id].is_alive():
                    return  

Um einen Doppelstart zu verhindern er gab ich dieese Zeile dazu.

Da wir aber auf Probleme stießen, wie der Sensor im Jintergrund lief, musste ich den Mess-Thread im hintergrund als Daemon laufen lassen

            stop_events[arbeitsplatz_id] = threading.Event()
            thread = threading.Thread(
                target=dauerhafte_messung,
                args=(arbeitsplatz_id, i2c_bus, i2c_address),
                daemon=True
            )
            sensor_threads[arbeitsplatz_id] = thread
            thread.start()

Dabei wird auch das Stop-Ergebnis gespeichert. Die Funktion stop_sensor ist dann dafür da um alles aufzuräumen.

            def stop_sensor(arbeitsplatz_id):
                if arbeitsplatz_id in stop_events:
                    stop_events[arbeitsplatz_id].set()
                if arbeitsplatz_id in sensor_threads:
                    sensor_threads[arbeitsplatz_id].join()
                    del sensor_threads[arbeitsplatz_id]
                    del stop_events[arbeitsplatz_id]
                    luxwerte.pop(arbeitsplatz_id, None)

Hierbei wird das Stop-Event gesetzt, somit wird der Mess-Thread beendet. Die Daten werden dann am Ende aus dem Dictionary gelöscht.

Die Luxwerte werden in der Funktion get_luxwerte als Text zurückgegeben. Es kommt dann auf den Zustand an.

            def get_luxwert(arbeitsplatz_id):
                wert = luxwerte.get(arbeitsplatz_id)
                if wert is None:
                    return "kein Sensor angeschlossen"
                elif wert == -1:
                    return "Sensorfehler"
                else:
                    return f"{wert} Lux"

Auch diese Datei musste ich dann wieder in die views.py miteinbinden.

          from .tageslichtsensor import start_sensor, stop_sensor, get_luxwert

start_sensor fügte ich dann in arbeitsplatz_buchen hinzu. stop_sensor zu arbeitsplatz_abmelden und für get_luxwerte musste ich eine neue Funktion anlegen.

          if "luxsensor" in arbeitsplatz:
                    sensor_info = arbeitsplatz["luxsensor"]
                    start_sensor(arbeitsplatz["id"], sensor_info["i2c_bus"], sensor_info["i2c_address"])


          def luxwert_aktuell(request):
              user_id = request.session.get("user_id")

              with open(arbeitsplaetze, "r") as file:
                  daten = json.load(file)

              for ap in daten["arbeitsplaetze"]:
                  if ap.get("user_id") == user_id and ap.get("status") == "belegt":
                      ap_id = ap["id"]
                      lux = get_luxwert(ap_id)
                      return JsonResponse({"lux": lux})

              return JsonResponse({"lux": "kein Sensor angeschlossen"})

Hierbei fügte ich wie in allen unserern Funktionen in dr views.py die überprüfung ob der Benutzer auch eingeloggt ist. Dann wird die Arbeitsplaetze.json geöffnet in der die Arbeitsplätze stehen.
In dieser Datei wird dann nach dem belegten Arbeitsplatz des Benutzers gesucht. Dann wird der aktuelle Luxwert des Arbeitsplatzes aufgerufen und zurückgegeben.

### Modell

Meine dritte Aufgabe war es ein Modell zu erstellen. Wir wollten unser Modell aus Holz bauen, also brauchten wir Geräte um das Holz zu bearbeiten und zusammenzufügen. Hier hat mir KI sehr geholfen, da wir eine Vision unserem vorgestellen Modell erzeugten. Dadurch konnte ich dann meinem Onkel zeigen wie das Holz geschnitten werden sollte. Mithilfe einer Holzsäge konnte er dann das Holz sägen. Zuhause habe ich es dann mit Leim und Schraubzwingen habe ich dann das Holz zusammengeklebt und Löcher hineingebohrt. In der letzten Woche vor der Projektmesse traf ich mich dann mit Stefan im Urban-Harbour um die LEDs zu verkabeln und mit unseren Raspberry Pis zu verbinden. Hier trat dann das Problem auf, dass mein Raspberry sich nicht mit unseren Hotspots verbinden wollte, egal was wir machten. Dadurch mussten wir dann mit einem arbeiten und konnten daher auch nur einen Tageslichtsensor verwenden und auch nicht alle geplanten Arbeitsplätze, da es einfach keine Pins mehr gab.

## Fazit

Dadurch dass wir bereits im vorherigen Semester ein Zeitbuchungssystem erstellten, waren einige Dinge hier leichter, da die Grundbasis schon bestand und wir einiges vom vorherigen Projekt nehmen konnten. Allerdings waren vorallem die Sensoren extrem schwer zu verbinden und korrekt einzusetzen. Duurch unsere gute Vorarbeitung und Einteilung der Aufgaben hat am Ende alles aber gepasst. Es war sehr interessant und hat viel Spaß gemacht so ein Tool zu entwickeln, da es diesesmal auch viel Anwendungsmöglichkeiten im tatsächlichen Leben haben kann.  Der Raspberry Pi war neu, ich konnte mich aber schnell mit ihn zurechtfinden.

