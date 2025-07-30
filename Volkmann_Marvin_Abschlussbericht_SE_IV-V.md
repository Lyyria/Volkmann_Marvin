# Abschlussbericht SE IV-V Desk-Sharig Toll Marvin Volkmann

## Organisation
Bereits vor dem Start des Projekts fingen wir an , uns mit dem Raspberry Pi ausanderzusetzen. Wir erstellten unsere Teams Gruppe. Da wir im vorherigen Projekt bereits mit dem eingebauten Kanban-Board gearbeitet haben, viel es uns leicht es diesesmal wieder zutun. Um unseren Code gegeinander auszutauschen, benutzten wir wiedr Github. Über den gesamten Februar machten wir uns also daran Wissen aufzubauen, damit wir bereits eine solide Wissensbasis zum Start des Projekts hatten. So wussten wir bereit auch früh was unser Projekt werden sollte. Am Start des Projekts haben wir uns dann in die unterschiedlichen Rollen eingeteilt, wobei wir es recht flexibel hielten um uns gengeseitig unterstützen zu können. Im Kanban-Boar dtrugen wir die einzlnen Teilaufgaben ein, beziehungsweise teilten alles grob auf in Frontend, Backend und GPIO. So hatte jeder eine gute Übersicht über eine einzelnen Aufgaben. Weitere Aufgaben konnten wir dazugeben und existierende abändern, falls es nötig war. Weitere Kategorien waren Wissens-Management, Planung/Vorbereitung, Einkauf Material und IT-Messe. Somit hatten wir eine klare Übersicht über alle möglichen Aufgaben.

![Screenshot 1](https://i.imgur.com/B1AZpYO.png).

Durch die Vorgabe der Sprints konnten wir die Aufgaben gut einteilen. Wir wollten im Sprint 1 hauptsächlich das Frontend und Materialien priorisieren. In Sprint 2 dann das Backend und GPIO Funktionen und in Sprint 3 dann das Modell und Probleme beheben. 

### Code

Den Code teilten wir wie bereits erwähnt auf Github. Es wurde zuerst jede Datei des Servers hochgeladen. Danach konnte dann jeder die Datei an der er arbeitete einfach überschreiben und die anderen konnten es sich so auf ihren eigenen Server ziehen. 

![Screenshot 2](https://i.imgur.com/KfvGl6L.png).

In den Ordner lagen dann die Templates, .views, .urls sowie die GPIO Funktionen.

## Meetings

Meetings wurden einmal in der Woche gemacht. Dabei wurden dann meistens Fragen geklärt, Ergebnisse besprochen und neue Aufgaben erstellt. Danach wurden meistens der Termin daes nächsten Meetings im aktuellen besprochen. Vorallem gegen Ende des Projekts hatten wir dann auch teilweise zwei Meetings in der Woche um die letzten Probleme zu klären.

## Eigenleistung

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

