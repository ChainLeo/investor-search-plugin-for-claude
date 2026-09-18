# Investor Search

**Build a sourced list of the investors in a market — and say honestly how complete it is.**

Ask an AI agent for the family offices in a country and you get the number you asked for. Ask
again tomorrow and you get an overlapping but different set, with no way to tell which firms are
new and no way to know what is missing.

This plugin replaces the number with a rule, separates real investors from the advisers who sell
to them, and writes files where every claim carries the page it came from.

## Install

```
/plugin marketplace add anthropics/claude-plugins-community
/plugin install investor-search@claude-community
```

Then ask in plain language:

```
find the family offices in Austria
who are the private investment groups in Poland
family offices in Germany that invest in software
enrich my investors.csv — fill in the missing websites and people
```

It needs a web search tool, a way to fetch pages, and `python3` for the memory store. No account,
no API key, no database, no subscription to anyone's data service.

## What it does differently

**It knows an investor from an adviser.** A search for "family office" in any language returns
mostly law firms, accountancies, private banks and company-formation agents selling family-office
*services*. They have the words in their name, a real site and a real address — and they pass
every check most tools apply.

> Measured on one market: a plain agent returned 88 rows. **24 of them were advisers, banks or
> consultancies** — firms that sell to family offices rather than invest. One company appeared
> twice under two names.

**It says what is proven and what is not.** Family offices come back as **two numbers, never one**:
*proven* (a page says the firm is a family office) and *likely* (only its name or a register entry
says so). A confidence score would hide that difference; two numbers cannot.

**It refuses to merge two firms into one row without proof.** Accents, legal forms and
transliteration variants are folded, but two names that fold to the same key are only merged when a
shared domain agrees — otherwise both are kept and the pair is flagged.

**Empty stays empty.** A field it cannot source is blank. It will not infer an email from a name
and a domain, or a city from a phone code.

**It finds the money where it actually sits.** In several markets the family capital is in
foundations and holding companies that publish nothing — Austrian *Privatstiftungen*, German
*Familienstiftungen*, Dutch STAKs. The company registry is searched as a first-class source, and a
firm with no website is not dropped: its registry entry carries the identity, and a second source
has to show an actual investment before it joins the list.

## What you get

One `<market>-investors.xlsx`. The first sheet is the firm list with a `source_links` column; the
others hold the evidence, the open queue, the round history and a ledger.

Six CSVs stay in the working folder — they are the memory, not the deliverable. Run it again next
month and it reads them, skips every firm already decided, and continues instead of repeating.

## People, and what is not collected

The `key_people` column holds a person's **name and business role only**, and only where a public
page names them in that role, with that page as the source. No personal email addresses, no direct
phone numbers, no home addresses, nothing inferred. If you need contact details, get them from the
person.

## What it will not do

- **No email addresses.** Almost no family office publishes one, and inventing them is the exact
  failure this exists to prevent.
- **No ranking, scoring or recommendation.** It is not investment advice.
- **No confidence scores.** Provenance labels replace them.
- **No judgement it cannot show you.** Every rejection is in a file with its reason.

## A run takes minutes

It searches until the results stop — six rounds in a row with no new firm, across different kinds
of source — and it tells you before it starts roughly how long that is, how approvals will work,
and what happens if you close the window. Say `use 10 rounds` for a quick scan. Say
`continue <market>` to carry on later.

## Licence

**MIT-0.** Use it, change it, redistribute it, sell work built with it. Attribution is not required.
