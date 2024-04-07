# DMI open data og aktuel vejr

Hent aktuel vejr fra DMI open data: https://opendatadocs.dmi.govcloud.dk/DMIOpenData

Det kræver du får en API nøgle og finder nærmeste vejrstation: https://www.dmi.dk/friedata/observationer. Vælg vejrstation og se aflæs StationsID længere nede på siden.

Hver 10. minut fås disse data ("Vejr" er ikke altid beskrevet):

![image](https://github.com/MaximusClavius/DMI-vejr/assets/103023823/bc79b91a-fc69-40c0-ab48-c21ac9287665)

Hver time fås disse data:

Når man har et API-nøgle og fundet relevant StationsID, så skal man gøre følgende i Home Assistant:
1) Lav en "command line sensor", som skal hente data hver 10. minut hos DMI
2) Lav en "template sensor", som skal fiske relevante data ud af JSON output'tet.
3) Lav kort i dashboard. Der er to eksempler her - en på 10. minuts data og en på time data.
