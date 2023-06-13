---
layout: post
title: Google Calendar の特定日付以前の公開範囲を一括で非公開に変える
# author: takapiro_99
# image: /huit_logo_white.png
# updated_at: 2021-10-04
published: true
---

```js
function run() {
  // Change the date range where you want this script to be executed
  const fromDate = new Date("2021-05-01");
  const toDate = new Date("2023-02-01");
  Logger.log("Events to be deleted starting: " + fromDate);
  Logger.log("Events to be deleted starting: " + toDate);

  // put your email address here, follow the prompts to give permissions to this script to access your calendar
  const calendarName = "<your email address>@gmail.com";

  const calendar = CalendarApp.getCalendarsByName(calendarName)[0];
  const events = calendar.getEvents(fromDate, toDate);
  Logger.log(`${events.length} events found`);

  for (let i = 0; i < events.length; i++) {
    const targetEvent = events[i];
    if (targetEvent.getVisibility() === CalendarApp.Visibility.PRIVATE) {
      continue;
    }
    targetEvent.setVisibility(CalendarApp.Visibility.PRIVATE);
  }

  Logger.log("done!");
}
```

人に Google Calendar を共有する際に、これより前の予定は見られたくないなあと思ったときは、ぜひお試しください！
