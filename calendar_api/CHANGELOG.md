<!-- https://developers.home-assistant.io/docs/add-ons/presentation#keeping-a-changelog -->

## 0.3.1

- Bump CalendarAPI to v0.1.8 to add more safety and prevent panics from gocal

## 0.3.0

- add OpenTelemetry configuration options for better observability
- remove unused environment variable exports in run script to clean up code
- update CALENDAR_API_VERSION to v0.1.7 and version to 0.3.0 for consistency

## 0.2.0

- Add configuration options for exporting opentelemetry traces

## 0.1.8

- Bump CalendarAPI to v0.1.4

## 0.1.7

- fix s6 overlay init
- Bump CalendarAPI to v0.1.3

## 0.1.5

- Re-structure the docker build process
- Use rootfs and bashio instead of the pre-build calendarapi docker container
- Label Docker container correctly

## 0.1.4

- Bump CalendarAPI to v0.1.2

## 0.1.2

- Use pre-build Docker container instead of building locally to improve install speed of the Add-On

## 0.1.1

- Small fixes

## 0.1.0

- Initial release
