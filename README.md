# DMI open data og aktuel vejr

Hent aktuel vejr fra DMI open data: https://opendatadocs.dmi.govcloud.dk/DMIOpenData

Det kræver du får en API nøgle og finder nærmeste vejrstation: https://www.dmi.dk/friedata/observationer. Vælg vejrstation og se aflæs StationsID længere nede på siden.

Hver 10. minut fås disse data ("Vejr" er ikke altid beskrevet):

![image](https://github.com/MaximusClavius/DMI-vejr/assets/103023823/bc79b91a-fc69-40c0-ab48-c21ac9287665)

Hver time fås disse data:

![image](https://github.com/MaximusClavius/DMI-vejr/assets/103023823/4ad8877c-e155-41fe-bcbb-b3695f440248)

## Implementering
Når man har et API-nøgle og fundet relevant StationsID, så skal man gøre følgende i Home Assistant:
1) Lav en "command line sensor", som skal hente data hver 10. minut hos DMI (se command-line)
2) Lav en "template sensor", som skal fiske relevante data ud af JSON output'tet.
3) Lav kort i dashboard. Der er to eksempler her - en på 10. minuts data og en på time data.

Bemærk: 
1) Det er forskelligt hvilke værdier der er tilgængelig for de forskellige vejrstationer
2) Attributten: weather er en kode til en tektuel beskrivelse, og den opdateres ikke altid
3) Template sensor overskriver værdierne hver eneste gang - også selvom der ikke er noget indhold. Tricket er at gemme forrige værdi, og opdater med den nye såfremt den findes. Det dette formål skal man bruge namespace i jinja2, fx
```
humidity_past1h: >
{% set ns = namespace(value = state_attr('sensor.current_weather', 'humidity_past1h')) %} # gem forrige værdi
{% for geometry in state_attr("sensor.local_weatherstation_dmi", "features") %}
  {% if geometry.properties.parameterId == 'humidity_past1h' %}
    {% set ns.value = geometry.properties.value %}                                        # gem den nye værdi
    {% break %}                                                                           # og ud af løkken
  {% endif %}
{% endfor %}
{{ ns.value }}                                                                            # giv sensor attributten den nye værdi
```
