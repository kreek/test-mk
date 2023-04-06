# Dates & Time

!!! warning "Requirement"
    - Dates and timestamps **must** follow the [ISO 8601 standard](https://www.iso.org/iso-8601-date-and-time-format.html).

!!! success "Guidance"
    - Dates and timestamps **should** be in [Coordinated Universal Time (UTC)](https://www.timeanddate.com/time/aboututc.html).
    - Timestamps **should** NOT [use offsets](#avoiding-offset--time-zone-errors).
    - Durations **should** follow the [ISO 8601 standard](https://en.wikipedia.org/wiki/ISO_8601#Durations).
    - Time Intervals **should** follow the [ISO 8601 standard](https://en.wikipedia.org/wiki/ISO_8601#Time_intervals).


## Dates

If your field only needs a date rather than a complete timestamp, use the ISO 8601 format `YYYY-MM-DD`.

This means the full year, month with leading zero, and day with leading zero, seperated by hyphens.

```json title="December 24th, 2049"
{
  "date": "2049-12-24"
}
```

```json title="January 2nd, 2049"
{
  "date": "2049-01-02"
}
```

If only the month is required, you may omit the day. In this case, always use the full year, and do not omit the hyphens to avoid confusion with the `YYMMDD` format.

```json title="February, 2049"
{
  "date": "2049-02"
}
```

## Timestamps

Timestamps must be in ISO 8601 format in UTC, also known as Zulu time, which is denoted with a trailing 'Z'.

As with dates above, hours, minutes, and seconds must use leading zeros. All times must use a 24 hour clock (military time).

```json title="December 12th, 2049, at 3:09 PM"
{
  "startTime": "2049-12-24T15::09::00Z"
}

```

### Avoiding offset & time zone errors

Use UTC time.

It can be tempting to localize timestamps using offsets. For example, an API could return appointment times in the time zone of the facility's location. There are several issues with this example.

- Offsets do not represent time zones. A time zone's offset can change with daylight savings time.
- End users may live in another time zone from the facility they visit.
- A VA system in another time zone may schedule a facility's appointments. Also, 13 states have more than one time zone.

UTC time has no offset and does not implement daylight savings time. Let your API be the source of truth for time and leave the local formatting up to the consuming application.

If you do need to capture or return the time zone for an event, use an additional field with a value of the full qualified name in the [tz database](https://www.iana.org/time-zones). An example would be `America/Los_Angeles`.

## Duration

Durations represent an amount of time, in the format `P[n]Y[n]M[n]DT[n]H[n]M[n]S`, where:


- P is the duration designator (for period) placed at the start of the duration representation.
    - Y is the year designator that follows the value for the number of calendar years.
    - M is the month designator that follows the value for the number of calendar months.
    - W is the week designator that follows the value for the number of weeks.
    - D is the day designator that follows the value for the number of calendar days.
- T is the time designator that precedes the time components of the representation.
    - H is the hour designator that follows the value for the number of hours.
    - M is the minute designator that follows the value for the number of minutes.
    - S is the second designator that follows the value for the number of seconds.

You do not need to include all the duration and time designators.

```json title="Three years, six months, four days, twelve hours, thirty minutes, and five seconds"
{
  "duration": "P3Y6M4DT12H30M5S"
}
```

```json title="Three years, six months"
{
  "duration": "P3Y6M"
}
```
```json title="Three years"
{
  "duration": "P3Y"
}
```

## Intervals

Intervals represent a range of time with specific start and end dates.

```json
{
  "interval": "2049-03-01T13:00:00Z/2049-05-11T15:30:00Z"
}
```
