# Awqat - Islamic prayer times for Emacs

Awqat is an Emacs package that calculates daily Islamic prayer times and provides optional mode-line display, Adhan playback, and desktop notifications.

## Features

- Calculate the six daily times (Fajr, Sunrise, Dhuhr, Asr, Maghrib, Isha)
- Multiple calculation presets (MWL, Umm al-Qura, Diyanet, MCW, and more)
- High-latitude adjustment support
- Mode-line countdown to the next prayer (`awqat-display-prayer-time-mode`)
- Optional Adhan playback at prayer time (`awqat-adhan-mode`)
- Optional desktop notifications and pre-notifications (`awqat-notification-mode`)
- Diary/Org Agenda integration

## Installation

Awqat is available on [MELPA](https://melpa.org/#/awqat).

### Install from source

Awqat currently requires Emacs **28.1+**.

```elisp
(use-package awqat
  :vc (:url "https://github.com/abougouffa/awqat"
       :rev :newest))
```

### Spacemacs

```elisp
dotspacemacs-additional-packages
'((awqat :location (recipe
                    :fetcher github
                    :repo "abougouffa/awqat")))
```

Then add `(require 'awqat)` in `dotspacemacs/user-config`.

### Doom Emacs

In `packages.el`:

```elisp
(package! awqat
  :recipe (:host github
           :repo "abougouffa/awqat"))
```

In your config:

```elisp
(use-package! awqat
  :commands (awqat-display-prayer-time-mode
             awqat-times-for-day)
  :config
  (setq calendar-latitude 44.2
        calendar-longitude 1.3
        awqat-mode-line-format " 🕌 ${prayer} (${hours}h${minutes}m) ")
  (awqat-set-preset-muslim-world-league))
```

## Quick setup

At minimum, set your location:

```elisp
(setq calendar-latitude 52.439
      calendar-longitude 13.436)
```

Then choose a preset (recommended):

```elisp
(awqat-set-preset-muslim-world-league)
```

Or configure values manually (example):

```elisp
(setq awqat-asr-hanafi nil
      awqat-fajr-angle -18.0
      awqat-isha-angle -17.0)
```

## Calculation methods and key variables

### Fajr / Isha

- Angle-based method: `awqat-fajr-angle`, `awqat-isha-angle`
- Offset-based method: `awqat-fajr-before-sunrise-offset`, `awqat-isha-after-sunset-offset`
- Switch methods with:
  - `awqat-use-angle-based-method`
  - `awqat-use-time-offset-method`

### Maghrib / Sunrise

- Default astronomical angle: `awqat-sunrise-sunset-angle` (constant)
- Optional Maghrib override: `awqat-maghrib-angle`
- Optional sunset offset mode (used by some presets): `awqat-maghrib-after-sunset-offset`

### Asr

- Use Hanafi method by setting `awqat-asr-hanafi` to non-nil.

### High latitudes

Use `awqat-set-preset-high-latitudes` with one of:

- `one-seventh-of-night`
- `one-third-of-night`
- `midnight`
- `angle-based`

Related variables:

- `awqat-high-latitudes-adjustment-method`
- `awqat-high-latitudes-adjustment-max-latitude`
- `awqat-high-latitudes-adjust-maghrib`

Compatibility aliases still exist for:

- `awqat-set-preset-midnight`
- `awqat-set-preset-one-seventh-of-night`

### Safety offsets

Set prayer safety offsets in minutes with `awqat-prayer-safety-offsets` in order:

`(Fajr Sunrise Dhuhr Asr Maghrib Isha)`

Example:

```elisp
(setq awqat-prayer-safety-offsets '(0.0 -1.0 2.0 0.0 1.0 0.0))
```

## Available presets

- `awqat-set-preset-midnight`: Obsolete compatibility wrapper (no docstring).
- `awqat-set-preset-one-seventh-of-night`: Obsolete compatibility wrapper (no docstring).
- `awqat-set-preset-diyanet`: Set the calculation method defined by Diyanet İşleri Başkanlığı, Turkey.
- `awqat-set-preset-diyanet-standard`: Set the calculation method to the standard Diyanet İşleri Başkanlığı, Turkey.
- `awqat-set-preset-muslim-pro`: Use the calculation method defined by the Muslim Pro app, non official.
- `awqat-set-preset-muslim-world-league`: Use the calculation method defined by the Muslim World League.
- `awqat-set-preset-karachi-university-of-islamic-sciences`: Use calculation method by Karachi University of Islamic Sciences (KUIS).
- `awqat-set-preset-umm-al-qura`: Use the calculation method defined by Umm al-Qura University, Makkah.
- `awqat-set-preset-egyptian-general-authority-of-survey`: Use the calculation method defined by the Egyptian General Authority of Survey.
- `awqat-set-preset-kuwait`: Use the calculation method used in Kuwait.
- `awqat-set-preset-institute-of-geophysics-university-of-tehran`: Use calculation method by the Institute of Geophysics, University of Tehran.
- `awqat-set-preset-jafari`: Use calculation method used by Shia Ithna-Ashari, Leva Institute, Qum.
- `awqat-set-preset-jakim`: Use calc method by Department of Islamic Development Malaysia (JAKIM).
- `awqat-set-preset-morocco`: Use the calculation method used in Morocco.
- `awqat-set-preset-taiwan`: Use the calculation method used in Taiwan.
- `awqat-set-preset-dubai`: Use the calculation method used in Dubai, UAE.
- `awqat-set-preset-gulf-region`: Use the calculation method used in some countries in the Gulf region.
- `awqat-set-preset-qatar`: Use the calculation method use in Qatar.
- `awqat-set-preset-spiritual-administration-of-muslims-russia`: Use calculation method by Spiritual Administration of Muslims, Russia (SAMR).
- `awqat-set-preset-french-muslims`: Use calculation method by the French Muslims.
- `awqat-set-preset-grande-mosquee-de-paris`: Use calculation method similar to one used by Grande Mosquée de Paris, France.
- `awqat-set-preset-isna`: Use calculation method by Islamic Society of North America (ISNA).
- `awqat-set-preset-portugal`: Use calculation method defined by Comunidade Islamica de Lisboa.
- `awqat-set-preset-jordan`: Use calculation method defined by the Ministry of Awqaf, Islamic Affairs and Holy Places, Jordan.
- `awqat-set-preset-high-latitudes`: Use the calculation METHOD used in higher latitudes.
- `awqat-set-preset-moonsighting-committee-worldwide`: Use calculation method defined by the Moonsighting Committee Worldwide (MCW).
- `awqat-set-preset-canada-13`: Use 13° calculation method used in some mosques in Canada.
- `awqat-set-preset-algeria`: Use calculation method by Ministry of Religious Affairs and Wakfs, Algeria.
- `awqat-set-preset-france-15`: Use 15° calculation method used in some mosques in France.
- `awqat-set-preset-france-18`: Use 18° calculation method used in some mosques in France.
- `awqat-set-preset-indonesia`: Use the calculation method defined by the Kementerian Agama Republik Indonesia.
- `awqat-set-preset-singapore`: Use the calculation method defined by the Majlis Ugama Islam Singapura.
- `awqat-set-preset-tunisia`: Use the calculation method used in Tunisia.
- `awqat-set-preset-uae`: Use the calculation method of UAE General Authority of Islamic Affairs And Endowments.
- `awqat-set-preset-canada-15`: Use 15° calculation method used in some mosques in Canada.
- `awqat-set-preset-canada-18`: Use 18° calculation method used in some mosques in Canada.

## Usage

### Show today's times

Run:

- `M-x awqat-times-for-day`

### Mode-line countdown

Enable:

```elisp
(awqat-display-prayer-time-mode 1)
```

Customize with `awqat-mode-line-format`, `awqat-update-interval`, `awqat-warning-duration`, and `awqat-danger-duration`.

### Adhan playback

Enable:

```elisp
(awqat-adhan-mode 1)
```

Configure:

- `awqat-adhan-file` (default file)
- `awqat-play-adhan-for-times` (per-prayer playback settings)
- `awqat-audio-player` (auto-detected from `ffplay`, `paplay`, `afplay`, `aplay`)

Stop currently playing Adhan:

- `M-x awqat-stop-adhan`

### Desktop notifications

Enable:

```elisp
(awqat-notification-mode 1)
```

Main settings:

- `awqat-notifications-enabled`
- `awqat-notifications-for-times`
- `awqat-notification-title`
- `awqat-notification-message-format`
- `awqat-pre-notifications-enabled`
- `awqat-pre-notification-minutes`
- `awqat-pre-notifications-for-times`
- `awqat-pre-notification-title`
- `awqat-pre-notification-message-format`
- `awqat-alert-style`

## Diary and Org Agenda integration

In your `diary-file`:

```text
%%(awqat-diary-fajr)
%%(awqat-diary-sunrise)
%%(awqat-diary-dhuhr)
%%(awqat-diary-asr)
%%(awqat-diary-maghrib)
%%(awqat-diary-isha)
```

To include diary entries in Org Agenda:

```elisp
(setq org-agenda-include-diary t)
```

## Notes on calculation methods

Awqat provides multiple methods and regional presets, but results can differ from local mosque/organization schedules. Always verify against your local authority when needed.

For high latitudes, special handling is supported but may still require manual method selection based on local jurisprudence and season.

Awqat implements the Moonsighting Committee Worldwide (MCW) method as a latitude/season-aware approach.
