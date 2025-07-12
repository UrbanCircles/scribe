Project Scribe

Simple, reliable, distraction free writer to nudge you towards a better life
story - by surfacing your receipts, one day at a time.

> [!TIP]  
> This is the base configuration of Scribe, to serve one straight forward,
> important use case: it turns the Scribe platform into a simple and reliable
> short message writer (simple by design). It works very well as is.
>
> However, if you wish, you can go beyond this and make this human-machine/
> human-computer interface truly your own.
>
> The hardware is capable, the design is easily adaptable, and the firmware is
> easy to develop/ scale in whichever direction your imagination leads you
> towards! I'm excited to see what you come up with!

Base Scribe: a simple, reliable, open-source system that helps you capture the
meaningful moments of your life. It leverages thermal printing to write a
tangible log of your life's story, daily achievements, thoughts, or memories on
a continuous roll of paper. It's designed to be a quiet companion that nudges
you to live more intentionally.

This project is born from the idea of "bringing receipts" for the life you lead/
towards the life you want to lead. It's about creating a physical artifact of
your journey. It is highly hackable, adaptable and scalable to fit your needs
and wants.

## Features of the "out-of-the-box" configuration (v1)

- **Thermal Printing:** Clean, permanent-enough tactile records - no mess, no
  dried ink
- **Standard Rolls:** Very cheap and available paper - e.g. couple of bucks for
  6 rolls
- **Minimalist Web Interface:** Sleek, distraction-free input with just a text
  box and submit button (and some confetti for that extra delight)
- **API Integration:** Integrate Project Scribe with your favourite tools like
  Apple Shortcuts, IFTTT, or your own custom scripts.
- **Offline Operation:** No cloud services required (but cloud capable) -
  everything works locally
- **Organic Design:** Loosely Inspired by Dalí and Gaudí, with flowing curves
  that hint at life's organic nature
- **Low Power:** Runs on a single USB port, no separate power supply needed
  (about 0.5W at idle)
- **Open Source:** Completely free, hackable, and customisable

![pic for profiles](https://github.com/user-attachments/assets/56afd51b-3560-419a-93f4-af315ba2968f)

## BOM

- D1 mini board (any Arduino board will do, but you will have to adjust the
  sketch)
- CSN-A4L thermal printer (any serial thermal printer might do, although I
  haven't tested for that - you may need to adjust power and pinout)
- Paper rolls (printer comes preloaded with one, and it'll last ages - here,
  you're looking for 57.5±0.5mm width and 30mm max diameter)

- 3D printer for the body (you may need some glue to fix the parts together - no
  screws required)
- Wires (for soldering and connecting components + USB wire to power the whole
  thing)
- 5V/ USB power supply capable of higher currents (only used during thermal
  printing)

Links to the 3D printed component files can be found in the
[Assembly](#assembly) section contains.

If you don't have a 3D printer but would like to build this, consider using the
PCBWay affiliate link: https://pcbway.com/g/86cN6v (discount to you + some small
help for the project).

### Affiliate links for the components

If you're so inclined, feel free to use the following affiliate links. They help
out the project :)

> [!NOTE] The components might be slightly different as listings always change
> silently - always check. If you notice any issues, please ping me to update
> the readme.

| Component                         | Amazon US               | Amazon UK               | AliExpress                                |
| --------------------------------- | ----------------------- | ----------------------- | ----------------------------------------- |
| Microcontroller (USB-C D1 Mini)   | https://amzn.to/4h2zQYO | https://amzn.to/4gRFgFe | -                                         |
| Thermal Printer (CSN-A4L)         | https://amzn.to/4kr5ksq | -                       | https://s.click.aliexpress.com/e/_opjoNrw |
| Paper Rolls, BPA-free (57.5x30mm) | https://amzn.to/4kpOREP | https://amzn.to/44nqGCg | -                                         |

> [!IMPORTANT] Do your own due diligence regarding thermal paper types - the
> thermal paper we handle everyday (e.g. through receipts from the grocery
> store, restaurants, takeaway, taxis, etc.) will contain BPA. When choosing
> your rolls for this, you should definitely go for BPA-free paper just to be on
> the safer side - the links provided are for BPA-free paper. If you can, go a
> step further and look for “phenol-free” paper. Three types that do not contain
> BPA or BPS and are competitively priced contain either ascorbic acid (vitamin
> C), urea-based Pergafast 201, or a technology without developers, Blue4est.

> [!NOTE] Some thermal paper is treated against fading - can last e.g. 35+
> years. If you're planning on using Scribe for archival purposes, consider ink
> fading when picking up the right rolls.

## Pin-out/ wiring during operation

The project uses SoftwareSerial to communicate with the printer, leaving the
hardware serial pins free.

| Printer Pin | D1 mini Pin | Power Supply Pin | Description      |
| ----------- | ----------- | ---------------- | ---------------- |
| TTL RX      | D3          | -                | D1 Mini Transmit |
| TTL TX      | D4          | -                | D1 Mini Receive  |
| TTL GND     | GND         | GND              | Common Ground    |
| Power VH    | -           | 5V               | Printer VIN      |
| Power GND   | GND         | GND              | Printer GND      |

Wires not listed in the table (e.g. TTL NC/ DTR) are unused andcan be removed.
Fewer wires => less clutter which is hugely helpful.

> [!IMPORTANT] Never power the printer directly from/ through the D1 Mini!
> You'll burn your microcontroller.
>
> **Only power the D1 Mini via one source** - either via USB during firmware
> flashing, or via the 5V pin during normal operation from the shared power
> supply.

## Microcontroller firmware

### Firmware Variables

Towards the top of the `firmware v1.ino` file, there are a few variables that
you'll need to change. Specifically:

- `wifiSSID` => your WiFi SSID (the name of your wifi network)
- `wifiPassword` => your WiFi password
- `utcOffsetInSeconds` => the UTC offset (in seconds) for your timezone.

To find the value you should put for `utcOffsetInSeconds`:

1. Go to
   [timeanddate.com's timezone page](https://www.timeanddate.com/time/zone/) and
   search for your location.
2. Find the "Ciurrent Offset" field, and note down the number there.
3. Multiply that number by 3600.

For example, if you're in New York, you'd search for "New York" on that page and
find that you're currently in UTC -4 (or UTC -5 if it's winter - yay timezones!)
Therefore your offset is `-14400` because `-4 * 3600 = -14400`.

### Flashing

Using the IDE of your choice (e.g. I use the
[standard Arduino IDE](https://www.arduino.cc/en/software/) and find that it
works well. Just add the modules for D1 mini + libraries). Make sure you update
the firmware variables before flashing it to the MCU (e.g. wifi details and
other preferences you might choose to tweak).

Ensure that everything is working **before** soldering, and squeezing your
components into the 3D printed shell!

> [IMPORTANT!] As mentioned above - do not power the printer through the D1 Mini
> and do not power the D1 mini via both the USB and its pins at the same time.

## Assembly

In each set, there are 2x 3D Printed components:

- The head unit (in which your MCU + Thermal Printer + wiring slot it)
- The neck/ leg (connects with the head and has a channel to elegantly route/
  feed your power cable through)

The printed components can be found at

**Printing considerations**

- The head has fillets, so you will most likely require supports
- Smaller line heights will produce better results
- The neck/ leg can be printed without any supports upright
- The components may vary in size slightly, so will the tolerances/ clearances -
  you may need to us glue to put the pieces together in case they're lose, or
  sandpaper in case they're too tight

**Assembly considerations**

- Make sure you route the wire through the neck/ leg of Scribe before you crimp
  the connectors
- Important: make sure each connection and wire is well isolated before you cram
  all the wiring into the head unit! You really don't want a short circuit
- Always to a test run before final assembly
- Do not glue the electrical components together, in case you need to service
  this later (you shouldn't need to glue them together)

## User guide for standard configuration (v1)

1. **Power On:** Connect the device to a beefy 5V USB power source. Wait a few
   moments for it to boot, connect to WiFi, and print its startup receipt.
2. **Get the IP Address:** The local IP address will be printed on the startup
   receipt.
3. **Start Scribing!**
4. **Look back at your story**
5. **Improve and scribe some more**

**Message format** The as-is firmware prints messages in the right orientation
for the roll of paper to naturally wind downwards, with word wrap. The first
line is the header, reminiscent of a calendar - date on black background. The
following lines are the message itself.

**Scribing through a web browser**

- The D1 Mini creates a local web server and the as-is configuration includes a
  minimalist, light web app
- Open a web browser on any device on the same network and navigate
  to `http://<IP_ADDRESS>`. Type your entry (up to 200 characters) and press
  Enter or click the "Send" button.
- This limitation is not the limitation of the printer/ hardware/ system, I just
  like it to keep the messages concise - you can change it in firmware (just
  like everything else around here)

**Scribing through the API**

- You can also send entries directly from a browser or script. For
  example: `http://<IP_ADDRESS>/submit?message=Went%20for%20a%20hike`
- This is particularly useful when running automations - it works straight out
  of the box
- Different to the web app, when using the API there is no character limit out
  of the box. In addition, you can also backdate your entries, by adding
  the `date` parameter: `http://<IP_ADDRESS>/submit?message=Finished%20the%20book&date=2025-07-04`

## Beyond the as-is: Ideas to Extend or Replace the As-Is Functionality

The combination of a WiFi-enabled MCU, a Thermal Printer and an API-ready web
server makes Scribe a powerful platform for all sorts of creative projects.

You can easily adapt the existing code to create a completely new experience.
Here are a few ideas to get you started:

- **Daily Briefing Printer:** Modify the code to fetch data from public APIs
  every morning. It could print:
  - Your first few calendar events for the day.
  - The local weather forecast.
  - A curated news headline from an RSS feed.
  - The word or quote of the day.
- **Task & Issue Tracker:** Connect it to your productivity tools (like Todoist,
  Jira, or GitHub) via their APIs or webhooks.
  - Print new tasks or tickets as they are assigned to you.
  - Print your most important tasks for the day each morning.
- **Kitchen Companion:** Place it in your kitchen to print:
  - Shopping lists sent from a family messaging app or a shared note.
  - A recipe of the day.
  - Measurement conversion charts on demand.
- **Tiny Message Receiver:** Create a unique, private messaging system. Family
  members could send short messages to the printer from anywhere, creating a
  physical message board.
- **Daily Dose of Fun:** Make it a source of daily delight by having it print:
  - A random joke or comic strip (like XKCD).
  - A "shower thought" from a Reddit subreddit.
  - A custom fortune cookie message.
- **Photo Booth Printer:** Extend the functionality to accept an image URL via
  the API, dither the image in the firmware, and print low-resolution, stylised
  versions of your photos.

## Troubleshooting

In my testing and usage, I found this setup to be extremely reliable (after all,
these printers are used in commercial settings). If the device is not printing
as expected, this may be because of several reasons, e.g.:

- incorrect wiring/ a short
- paper not inserted correctly
- current on offer is not high enough

Thermal Printer Manual, in case you need to look into things further:
https://www.manualslib.com/manual/3035820/Cashino-Csn-A4l.html

## Disclaimer

I've done my best to document everything accurately - however, there might be
mistakes. If you see them, or opportunities to improve, please open an issue.  
This is an open-source project given for free, with no warranties or guarantees.
It assumes a level of proficiency with electronics, assemblies, engineering,
etc. Do your own due diligence - it's your responsibility. Stay safe. Stay
productive. Work with what you have. Make the world a better place.
