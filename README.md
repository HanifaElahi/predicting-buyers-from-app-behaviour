# Predicting buyers from app behaviour

New users arrive every day, installing the mobile app or signing up on the web. Some are ready to buy; most are just looking. There was no clear way to tell which new users were worth reaching first. This case study walks through how I used app activity to find what buyers do differently from everyone else, and how that became a weekly way to spot likely buyers early.

**At a glance**

- Three behaviours made a user roughly twice as likely to pay. All three show commitment rather than activity.
- Several actions that sound like buying intent, like clicking to buy, turned out to be just as common among users who never paid.
- Buyers fell into four clear patterns, and about 4 in 10 paid almost straight after joining.
- Every week, new users were sorted into five tiers. More than half were taken off the call list.

---

## The problem

Every new user leaves a trail of activity: what they opened, what they tried, how often they came back. Somewhere in that trail is the difference between a user who's about to pay and one who never will. But on the surface they look much the same.

Without a way to tell them apart, follow-up effort went to whoever happened to look active, and a lot of it landed on people who were never going to buy.

So the question was: from app activity alone, can we tell which new users are likely to pay, and which actions show it most reliably? Answering that meant working through a few problems first.

---

## Why this was hard

There was no data on how users found the app, so everything had to come from behaviour alone. And the behaviour data wasn't ready to use. The same action was logged under different names on mobile and on web, and some users left a handful of events while others left thousands.

The bigger trap was looking only at buyers. Anything buyers do often looks like a signal, until you check how often non-buyers do it too. So the analysis was built around a comparison.

---

## How I approached it

I worked with two groups, using everything they did on the mobile and web app:

- **Users who paid**, with their full journey from joining to payment
- **Users who didn't pay**, who joined over the same months and had at least a month to buy

If both groups do something, it can't tell us who will pay. Only actions that buyers do much more often are worth acting on. That comparison became a scoring process.

---

## What I built

A scoring process that gives every new user four things:

| Field | Description |
| --- | --- |
| Tier | The recommended follow-up |
| Buyer type | The behaviour pattern it most closely matches |
| Priority score | Its position within the tier |
| Reason | A short, plain-language explanation of why it was flagged |

![How a new user moves through the scoring](assets/scoring-pipeline.png)

It runs in seven steps.

---

## How it works

1. **Make the data readable.** Clean up events across mobile and web, and sort every action into engagement, buying steps or navigation.
2. **Describe each user.** Build the same activity profile for buyers and non-buyers.
3. **Look for patterns among buyers.** Group buyers by how they behaved before paying.
4. **Check against users who didn't pay.** Keep only behaviours that are clearly more common among buyers.
5. **Find the signals that matter.** Separate commitment from plain activity.
6. **Turn signals into action.** Five tiers, a priority score and a reason line for every new user.
7. **Run it every week.** Apply the tiers to new users who haven't paid.

The full walkthrough is in [`model/method.md`](model/method.md). Running those steps turned up a few clear findings.

---

## What I found

**People buy in two waves.** About half of buyers paid within a week of joining. About 3 in 10 came back six months or more later, as the exam got close. Very little happened in between. So the first days after joining matter most, and users who go quiet aren't necessarily lost.

**Buyers come in four types.** About 4 in 10 barely used the app and paid within hours. Something before the app had already sold them. The rest either asked for help and paid within days, used the product properly for a few months, or used everything heavily for over a year.

![Four ways users buy](assets/buyer-types.png)

**The comparison changed the answer.** Asking for help looked like a strong signal among buyers, until it turned out non-buyers did it almost as often. The same was true of clicking to buy, joining free events and general browsing.

**Commitment predicts buying; activity doesn't.** Three kinds of behaviour held up, each making a user roughly twice as likely to pay: serious use of the product, reaching for paid content, and getting to checkout.

![Which behaviours set buyers apart](assets/signals.png)

**Even good signals aren't guarantees.** About 9 in 10 users who showed one still didn't pay. A flagged user is about twice as likely to buy, not certain to.

Those findings changed how follow-up worked.

---

## What changed

Follow-up no longer depended on who looked active. Each week, new users came out sorted into tiers, with a reason for every one:

- a small share came out HOT and were followed up within a day or two
- more than half came out COOL and were taken off the call list entirely
- every HOT user went into one running list, so the busy season starts with a ready pool of likely buyers

![Five tiers, each with a clear action](assets/tiers-funnel.png)

It also showed what's still missing.

---

## What I'd check next

- **A fairer comparison.** Buyers' activity stops when they pay, while non-buyers had at least a month. Comparing both over the same first days after joining would make the signals more reliable.
- **Where the quick buyers come from.** About 4 in 10 buyers can't be spotted from behaviour because they barely use the app. Data on how users found the app would close that gap.
- **Follow-up outcomes.** Recording whether flagged users were reached, and what happened, would show which tiers really convert.
- **A proper test.** Splitting new users between this scoring and the usual approach for a few weeks would show how much it actually helps.

---

## Takeaways

- Always compare against non-buyers. Several "signals" only looked real until I saw how often non-buyers did the same thing.
- Look for commitment, not activity. Effort and steps towards paying predicted buying. Browsing didn't.
- A lot of buying starts outside the app. About 4 in 10 buyers paid almost straight away, so something before the app likely sold them.

---

## Built with

Here's what the analysis runs on:

- **SQL** for pulling app activity across mobile and web
- **Python** for cleaning events, building user profiles, grouping buyers and scoring new users
- **Claude Code** for writing and testing the analysis and the weekly scoring script
- **Google Sheets** for the weekly lists, which were added automatically after each run

---

## Repo structure

```
├── README.md
├── assets/
│   ├── buyer-types.png
│   ├── data-cleanup.png
│   ├── scoring-pipeline.png
│   ├── signals.png
│   └── tiers-funnel.png
└── model/
    └── method.md
```

---

*Note: Company details, product features and figures are generalised. The approach and findings are from the actual analysis.*
