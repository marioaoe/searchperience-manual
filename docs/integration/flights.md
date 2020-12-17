## Flights CSV 

!!! info
    The language in which the flight information like `city name` or `country name` should be predefined.

CSV, Comma Separated Values (strict form as described in RFC 4180)

* It is a delimited data format that has fields/columns separated by the comma character %x2C (Hex 2C) records/rows/lines separated by characters indicating a line break. 
* RFC 4180 stipulates the use of CRLF pairs to denote line breaks, where CR is %x0D (Hex 0D) and LF is %x0A (Hex 0A). 
* Each line should contain the same number of fields. 
* Fields that contain a special character (comma, CR, LF, or double quote), must be `"escaped"` by enclosing them in double quotes (Hex 22). 
* The header will contain names corresponding to the fields in the file and should contain the same number of fields as the records in the rest of the file.

## Airlines

For airline clients one should include the arrival and departure information of the flight as it allows users for a more flexible search. Airline specific information is not necessary.

| Field name             | Description                                                        | Example                    |
|------------------------|--------------------------------------------------------------------|----------------------------|
| flightNumber           | Primary flight number.                                             | `AB0948`                   |
| codeShareFlightNumber  | Comma separated list of codeshare flights.                         | `AB045,AB124`              |
| scheduledDepartureDate | ISO 8601 complete calendar date in format "YYYY-MM-DD"             | `2020-11-01`               |
| scheduledDepartureTime | ISO 8601 time with offset of the scheduled departure.              | `15:45+02:00`              |
| departureArea          | Area from where the flight is departing.                           | `2`                        |
| departureTerminal      | Terminal from which the flight is departing.                       | `2`                        |
| departureGate          | Gate from where the flight is departing.                           | `34D`                      |
| departureAirportCode   | IATA code of the departure airport.                                | `PBI`                      |
| departureAirportName   | Name of the arrival airport, please avoid using the word `Aiport`. | `Palm Beach International` |
| departureCityName      | Name of the departure city.                                        | `Palm Beach`               |
| departureCountryCode   | ISO alpha-2 country departure code.                                | `US`                       |
| departureCountryName   | Name of the departure country.                                     | `United States of America` |
| scheduledArrivalDate   | ISO 8601 complete calendar date in format "YYYY-MM-DD"             | `2020-11-01`               |
| scheduledArrivalTime   | ISO 8601 time with offset of the scheduled arrival.                | `07:45+02:00`              |
| arrivalArea            | Area to which the flight is arriving.                              | `3`                        |
| arrivalGate            | Gate to which the flight is arriving.                              | `2A`                       |
| arrivalTerminal        | Terminal to which the flight is arriving.                          | `1`                        |
| arrivalAirportCode     | IATA code of the arrival airport.                                  | `LAX`                      |
| arrivalAirportName     | Name of the arrival airport, please avoid using the word `Aiport`. | `Los Angeles`              |
| arrivalCityName        | Name of the arrival city.                                          | `Los Angeles`              |
| arrivalCountryCode     | ISO alpha-2 arrival country code.                                  | `US`                       |
| arrivalCountryName     | Name of the arrival country.                                       | `United States of America` |

### Example Airlines CSV

[airline.csv](../assets/files/airline.csv)

## Airport

For airport client flights are bounded to arrivals or departures. If necessary Searchperience will handle the transformation into a complete flight route.

| FieldName             | Description                                                       | Example            |
|-----------------------|-------------------------------------------------------------------|--------------------|
| flightNumber          | Primary flight number.                                            | `AB0948`           |
| codeShareFlightNumber | Comma separated list of code share flight numbers.                | `AB045,AB124`      |
| scheduledDate         | ISO 8601 complete calendar date in format "YYYY-MM-DD"            | `2020-11-01`       |
| scheduledTime         | ISO 8601 time with offset arrival/departure (depend on direction) | `15:45+02:00`      |
| area                  | Area of arrival or departure.                                     | `1`                |
| terminal              | Terminal which the flight arrives or departs.                     | `1`                |
| direction             | Arrival `A` or Departure `D` flight.                              | `A`                |
| gate                  | Gate of arrival or departure.                                     | `2D`               |
| airlineCode           | IATA airline code.                                                | `BA`               |
| airlineName           | Complete name of the airline.                                     | `British Airways`  |
| airportCode           | IATA code of the airport.                                         | `SIN`              |
| airportName           | Name of the airport, please avoid using the word `Aiport`.        | `Singapore Changi` |
| cityName              | Name of the city.                                                 | `Singapore`        |
| countryCode           | ISO alpha-2 country departure code.                               | `SG`               |
| countryName           | Name of the departure country.                                    | `Singapore`        |
    
### Example Airport CSV

[airport.csv](../assets/files/airport.csv)