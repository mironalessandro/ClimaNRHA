# ClimaNRHA
Gestione del clima con Nodered 

Questo è un flow che gestisce appunto il clima in maniera automatica in base alla temperatura Thom. Fa inoltre un check sulle finestre aperte (opzionale) 

## Prerequisiti

  - Sensore di Umidità / Temperatura 
  - Entità climate
  - Una variabile che indichi se una delle finestre è aperta (opzionale. Se non volete usare questo parametro basta che la variabile input_boolean.climate_check_windows sia off)
  - Una variabile allarme che nel mio caso indica se qualcuno e presente o no in casa.

### Inoltre abbiamo bisogno di queste variabili:

input_boolean.automatic_climate --> serve ad attivare o disattivare l'automazzione 
input_boolean.climate_check_windows --> come spiegato sopra serve a gestire accensione e spegnimento in base alle finestre aperte
input_number.thom_maximum --> Indice di Thom massimo al quale far partire il clima
input_number.thom_minimum --> Indice di Thom minimo al quale far spegnere il clima
input_number.thom_minimum_nobody_home --> Indice di Thom massimo al quale far partire il clima quando nessuno è a casa
input_number.thom_maximum_nobody_home --> Indice di Thom minimo al quale far spegnere il clima quando nessuno è a casa
input_number.gradi_cold --> Gradi celsius in caso di accensione del clima in modalità "Cold"
input_number.gradi_dry --> Gradi celsius in caso di accensione del clima in modalità "Dry"
input_number.limite_humidity_dry_or_cold --> Percentuale per accendere in modalità dry o cold.

# Indice di THOM

sensor:
  - platform: template
    sensors:
      indice_thom:
        friendly_name: "Indice Thom"
        value_template: "{{ (states('sensor.temp')|float - (0.55-0.0055 * states('sensor.hum')|float)*(states('sensor.temp')|float - 14.5))|round(1) }}"
        unit_of_measurement: 'index'

In questo flow sono stati usati 2 indici di Thom uno per piano.

http://www.meteo.ing.unibo.it/Indice_di_Thom.htm

