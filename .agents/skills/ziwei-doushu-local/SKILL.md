---
name: ziwei-doushu-local
description: Use when the user asks for a reproducible local Zi Wei Dou Shu chart, its structured output, or to consider Zi Wei as one optional cultural lens in the 中医老李观时局 workflow. Requires complete birth inputs and explicit calendar/time boundary settings; never infer missing data.
---

# Zi Wei local engine adapter

Use the installed upstream engine through the local adapter. This Skill is separate from the upstream checkout and from the existing `nihaixia` and `zhongyi-laoli-content` Skills.

## Responsibilities

- This Skill: run deterministic local Zi Wei chart calculation and preserve its JSON output.
- `nihaixia`: consult relevant teaching material and traditional explanations, with source references. Do not invent quotes or copy restricted course text into commercial content.
- `zhongyi-laoli-content`: verify current news, apply A/B/C/D source tiers, maintain 中医老李 voice, and draft X posts only after user selects an angle or requests a draft.
- Zi Wei is not Bazi. This engine does not add physiognomy or Wu Yun Liu Qi.

## When to run the engine

A direct request such as “本地紫微排盘” can invoke it. For “用观时局流程分析这条新闻”, research and verify the news first. Only consider Zi Wei if the user explicitly asks and a specific person's reliable birth data is necessary and available. Never use an event timestamp as a birth time, a personal chart as a national/market chart, or a chart to infer a real person's health, virtue, intelligence, criminality, or inevitable outcome.

## Required input gate

Before running, collect and confirm all fields:

- `calendarType`: `solar` (公历) or `lunar` (农历)
- `year`, `month`, `day`; and for lunar, explicit `isLeapMonth`
- known `clockHour`, `clockMinute`; unknown time means stop, do not guess a birth hour
- `birthplace` (text) and `timeZoneId` (for example `Asia/Shanghai`), `utcOffsetMinutes` (standard offset, east positive); the adapter does not geocode
- `dstAdjustmentMinutes`: user-confirmed clock shift to standard time; normally 0. The engine does not auto-detect historical DST/timezone rules. If the requested shift crosses midnight, stop and ask the user to confirm a corrected date/time.
- `gender`: engine input `male` or `female`
- `lateZiRule`: explicitly select `next-day` or `current-day`
- `leapMonthPolicy`: explicitly select `split`, `next-month`, `prev-month`, or `origin-month`
- `applyTrueSolarTime`: explicitly true/false; if true, require a confirmed longitude. The engine uses its longitude correction and NOAA equation-of-time approximation; it does not fetch coordinates.

Do not use the user's own or historical personal birth information unless they provide it for this request. Test only with clearly labelled fictional data.

## Local invocation

Call:

`C:\Users\琦琦弯弯历险记\Documents\Codex\ziwei-doushu-adapter\bin\invoke-ziwei-chart.ps1 -Json '<JSON>'`

For larger JSON, pass it through stdin. Read the actual stdout and require `ok: true`. Preserve these fields in the result: `rawUserInput`, `timeAndCalendarConversion`, `appliedRules`, `engine`, `chart`. Do not have a language model guess or edit star placements.

Output calculation separately from interpretation. Calculation is only a reproducible result within a traditional Zi Wei system, not scientific validation or medical/financial advice.

## Current affairs and writing

Apply `zhongyi-laoli-content`'s workflow and source verification first. State observed facts separately from traditional interpretation and the account holder's judgment. Clearly mark traditional interpretations as cultural frameworks and explain assumptions, uncertainty, contrary evidence, and what would change the judgment. Do not force a chart onto unrelated news. Do not impersonate Ni Haixia or attribute AI analysis to him.

## Version/update handling

The upstream project is kept separately at `C:\Users\琦琦弯弯历险记\Documents\Codex\ziwei-doushu-upstream`; the adapter and this Skill live elsewhere. Before upstream updates, inspect the new diff, licenses and tests; pin the resulting commit in adapter `install.json`; rerun the relevant tests. Keep this Skill and existing account-writing Skill untouched by upstream updates. Reinstall/reload Codex Skills after updates if the desktop does not discover changes automatically.
