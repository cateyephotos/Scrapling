# Brainwave Entrainment App — Product & Technical Specification

> **Status:** Draft v0.1
> **Last updated:** 2026-06-19
> **Owner:** Product
> **Audience:** Product, Design, Engineering, QA

---

## 1. Overview

A mobile application that induces **brainwave entrainment** through rhythmic
audio (binaural beats and related techniques) to help users reach a targeted
mental state. The app lets users select frequencies within the standard
brainwave bands, fine-tune them in **1 Hz steps**, control how long they spend
at each frequency, and chain frequencies into **programs** so they can run
controlled head-to-head comparisons and discover which exact frequency works
best *for them*.

The defining characteristic of the product is **iterative self-experimentation**:
it is not a passive playback library, but a tool for finding and applying the
single most effective frequency (and dose/duration) for a desired brain state.

### 1.1 Example user flow

> Run **5 Hz for 10 minutes**, then **6 Hz for 5 minutes**, to determine which
> frequency best induces the target state.

---

## 2. Goals & Non-Goals

### 2.1 Goals

- Let users explore and apply specific frequencies across the four core
  brainwave bands.
- Support **1 Hz increments/modulation** so users can scan closely spaced
  frequencies within a band.
- Let users control **duration per frequency** to test dose/exposure effects.
- Let users **build, save, run, and compare programs** (sequences of
  frequency + duration steps).
- **Log session parameters and outcomes** so protocols can be compared over time.
- Present **session results** so users can judge which frequencies/programs were
  most effective for the intended state.

### 2.2 Non-Goals (v1)

- Clinical diagnosis, treatment, or any medical claim.
- Closed-loop neurofeedback via EEG hardware (tracked as a future option in
  §11; v1 relies on self-report).
- Social/sharing network features.
- Desktop or web clients (mobile-first; deferred).

---

## 3. Target Users & Use Cases

| User | Primary need |
|------|--------------|
| Self-experimenter / biohacker | Find the exact frequency that produces a target state |
| Focus/productivity seeker | A reliable beta/gamma protocol for concentration |
| Relaxation/sleep seeker | A reliable alpha/delta protocol for calm or sleep |
| Curious newcomer | Guided presets that work out of the box |

**Core use cases**

1. Pick a band and a specific frequency, set a duration, and run a single session.
2. Build a multi-step program (e.g. 5 Hz / 10 min → 6 Hz / 5 min) and run it.
3. After a session, rate the outcome and review past sessions to compare.
4. Iterate: adjust frequencies/durations, re-run, and converge on a personal optimum.

---

## 4. Domain Concepts

### 4.1 Brainwave bands

The app covers the four core bands called out by the product, plus the
**1 Hz modulation** capability that applies across all of them. Exact band edges
are configurable; the defaults below are conventional.

| Band  | Conventional range | Associated states (informational) |
|-------|--------------------|------------------------------------|
| Delta | 0.5 – 4 Hz         | Deep sleep, restoration            |
| Alpha | 8 – 12 Hz          | Relaxed, calm, light meditation    |
| Beta  | 12 – 30 Hz         | Alert, focused, active thinking    |
| Gamma | 30 – 100 Hz        | High-level cognition, peak focus   |

> **Note:** The product summary explicitly names alpha, beta, delta, and gamma.
> Theta (4–8 Hz) is *not* in the v1 core band list, but because the engine
> supports arbitrary frequencies and 1 Hz stepping, the 4–8 Hz region (e.g. the
> 5 Hz/6 Hz example) is still reachable as raw frequency input. Whether to
> surface Theta as a named band is an open question (§12).

### 4.2 Key terms

- **Frequency** — the target entrainment beat frequency in Hz (the difference
  between left/right carrier tones for binaural beats).
- **Carrier frequency** — the base audible tone (e.g. 200 Hz) on which the beat
  is constructed. Configurable; affects comfort, not the entrainment target.
- **Step** — one `{frequency, duration}` unit within a program.
- **Program** — an ordered list of steps, optionally with transitions/ramps.
- **Session** — a single execution of a single frequency or a program, with a
  start time, end time, parameters, and an outcome rating.
- **Modulation / ramp** — a smooth transition in frequency between steps rather
  than an abrupt jump.

---

## 5. Functional Requirements

### 5.1 Frequency control & modulation

- **FR-1** Users can select a **central band** (Delta / Alpha / Beta / Gamma).
- **FR-2** Users can select a **specific frequency within that band**.
- **FR-3** Frequency can be adjusted in **1 Hz increments** (scan closely spaced
  frequencies). Sub-Hz fine steps (e.g. 0.5 Hz) are a configurable option
  (§12, open question).
- **FR-4** Users can set the **carrier frequency** (default provided; advanced
  setting).
- **FR-5** The engine supports **single-frequency sessions** and
  **multi-frequency ramps/sequences**.
- **FR-6** Selecting a frequency outside a band's defined range is allowed (raw
  entry) but clearly indicated.

### 5.2 Duration control

- **FR-7** Users can set a **duration per frequency/step** (e.g. minutes/seconds).
- **FR-8** Variable exposure times are supported to test **dose effects**.
- **FR-9** Reasonable min/max and default durations are enforced/suggested.

### 5.3 Programs & sequencing

- **FR-10** Users can **build a program**: an ordered list of
  `{frequency, duration}` steps.
- **FR-11** Users can **reorder, edit, duplicate, and delete** steps.
- **FR-12** Users can choose **abrupt step changes or ramped transitions**
  between steps.
- **FR-13** Users can **save, name, and re-run** programs.
- **FR-14** The app ships with **starter presets** (e.g. Focus, Relax, Sleep) the
  user can run as-is or clone and edit.
- **FR-15** Example program (5 Hz/10 min → 6 Hz/5 min) is expressible and
  runnable.

### 5.4 Playback / transport

- **FR-16** Start, pause/resume, stop, and skip-to-next-step controls.
- **FR-17** A clear display of: current frequency, current band, step index,
  time remaining in step, and total time remaining.
- **FR-18** Background playback (continues with screen off / app backgrounded).
- **FR-19** Audio respects system volume and a safe in-app volume cap.
- **FR-20** Headphone detection with a prompt, since binaural beats require
  stereo separation (left/right) to work.

### 5.5 Logging & results

- **FR-21** Every session logs its **parameters** (frequencies, durations,
  carrier, transitions, program id) and **timestamps**.
- **FR-22** After a session, the user can record an **outcome rating** of how
  well the target state was reached (see §6 for the measurement model).
- **FR-23** Users can attach optional **notes/tags** (e.g. "tired", "caffeine").
- **FR-24** A **history view** lists past sessions with their parameters and
  ratings.
- **FR-25** A **comparison view** lets users compare outcomes across frequencies
  or programs to judge which were most effective for a target state.
- **FR-26** Data can be **exported** (e.g. CSV/JSON) for the user's own analysis.

---

## 6. Measurement & Feedback (Effectiveness)

The product summary notes that measurement/feedback was raised conceptually but
not specified, and that it will determine how reliably users can identify optimal
frequencies. This section makes the v1 decision explicit and leaves room to grow.

### 6.1 v1: structured self-report

- After each session/step, prompt for a **target-state rating** (e.g. 1–5 or
  0–10) — "How well did you reach *<target state>*?"
- Optionally capture a **before/after** rating to measure *change*, not just
  end-state.
- Optional structured tags (mood, alertness, calmness) and free-text notes.
- Outcomes are stored against the exact session parameters so the comparison
  view can rank frequencies/programs by average rating, holding the target
  state constant.

### 6.2 Why self-report first

It requires no extra hardware, ships in v1, and is sufficient to drive the core
iterative loop (run → rate → compare → refine). The reliability of conclusions
depends on the user's consistency; the app should encourage consistent rating
conditions (e.g. same scale, same target state) and surface sample sizes.

### 6.3 Future: objective signals (deferred)

EEG wearables, HRV, reaction-time mini-tasks, or other behavioral proxies could
provide objective effectiveness measures. These are **out of scope for v1** but
the data model (§9) should not preclude adding measurement sources later.

---

## 7. User Experience / Key Screens

1. **Home / Quick start** — pick a band, jump into a default frequency, or
   resume a recent program.
2. **Frequency tuner** — band selector, frequency control with ±1 Hz stepping,
   duration setter, carrier (advanced), and a "Start" action.
3. **Program builder** — add/edit/reorder steps; choose transitions; save & name.
4. **Now playing / Session** — current frequency & band, step progress, time
   remaining, transport controls.
5. **Post-session rating** — capture outcome rating, optional before/after,
   tags, notes.
6. **History** — chronological list of sessions with parameters + ratings.
7. **Compare** — group/sort sessions by frequency or program; show
   average outcome and sample size for a chosen target state.
8. **Settings** — default carrier, volume cap, band edge configuration,
   units, safety/disclaimer.

---

## 8. Audio Engine Requirements

- **AE-1** Generate **binaural beats**: two carrier tones with a left/right
  frequency difference equal to the target beat frequency. (Stereo/headphones
  required.)
- **AE-2** Architected so additional entrainment methods (e.g. isochronic tones,
  monaural beats) can be added later without changing the program/session model.
- **AE-3** Smooth, click-free transitions between steps (ramped or crossfaded)
  to avoid audible artifacts and discomfort.
- **AE-4** Accurate, drift-free timing for step durations and total session
  length, including while backgrounded.
- **AE-5** Continuous generation for long sessions without buffer underruns,
  with low battery impact.
- **AE-6** Per-platform native audio (e.g. iOS AVAudioEngine / Android
  Oboe/AAudio) behind a shared interface.
- **AE-7** Frequency/duration parameters validated before playback (within
  engine-supported ranges).

---

## 9. Data Model (conceptual)

```
Band
  id, name, minHz, maxHz, defaultHz, associatedState

Step
  id, frequencyHz, durationSeconds, transition (abrupt | ramp), carrierHz?

Program
  id, name, description, steps[], createdAt, updatedAt, isPreset

Session
  id, programId?            // null for ad-hoc single-frequency runs
  startedAt, endedAt, completed (bool)
  resolvedSteps[]           // exactly what played, incl. carrier & transitions
  targetState               // the state the user was aiming for

Outcome
  sessionId, ratingScale, ratingBefore?, ratingAfter, tags[], notes
  measurementSource (selfReport | <future: eeg | hrv | task>)

UserSettings
  defaultCarrierHz, volumeCapDb, units, bandConfig, acknowledgedSafety
```

Design intent: **Outcome.measurementSource** is included from day one so that
objective signals can be added later without migrating the schema.

---

## 10. Non-Functional Requirements

- **Platforms:** iOS and Android (mobile-first). Cross-platform vs. native is an
  open decision (§12).
- **Offline-first:** core generation, playback, and logging work without
  connectivity. Sync/backup is optional and deferred.
- **Latency:** start playback within a small, perceptibly-immediate delay.
- **Battery:** long sessions (30–60+ min) should have modest battery impact.
- **Accessibility:** large tap targets, screen-reader labels, haptic/visual
  cues, no reliance on color alone.
- **Privacy:** session/outcome data is personal and stored locally by default;
  any cloud sync is opt-in and disclosed.
- **Reliability:** sessions must not silently stop; recover gracefully from
  interruptions (calls, route changes, unplugged headphones).

---

## 11. Safety, Compliance & Disclaimers

- **Not a medical device.** Display a clear disclaimer; make no clinical claims.
- **Photosensitive/seizure caution:** include a standard warning and advise
  users with epilepsy or relevant conditions to consult a professional;
  recommend stopping if discomfort occurs.
- **Hearing safety:** enforce a volume cap and warn against extended high-volume
  listening.
- **Headphones notice:** binaural beats require stereo headphones to function;
  prompt accordingly.
- **App store compliance:** wellness-app positioning; review medical-claim and
  health-data policies for both stores before launch.

---

## 12. Open Questions

1. **Theta band:** the core four are alpha/beta/delta/gamma, but the example uses
   5–6 Hz (theta range). Do we expose Theta as a named band, or only reach it via
   raw frequency entry?
2. **Step granularity:** is 1 Hz the *only* step, or do we also offer finer
   (e.g. 0.5 Hz) steps for sub-Hz scanning?
3. **Rating scale:** 1–5 vs. 0–10; single end-state vs. before/after delta as the
   default.
4. **Entrainment method:** binaural-only for v1, or also isochronic/monaural at
   launch (affects "headphones required" UX)?
5. **Tech stack:** cross-platform (Flutter/React Native) vs. fully native — given
   the precise real-time audio timing requirements.
6. **Carrier defaults & exposure:** recommended default carrier frequency and any
   suggested max single-frequency exposure for comfort/safety.
7. **Effectiveness analytics:** how much statistical guidance to give users
   (e.g. sample-size hints, simple averages) without implying clinical validity.

---

## 13. Phasing (suggested)

- **Phase 1 — Core loop:** single-frequency tuner with band selection, 1 Hz
  stepping, duration, binaural playback, basic logging + self-report rating,
  history.
- **Phase 2 — Programs & comparison:** program builder, transitions/ramps,
  presets, comparison view, export.
- **Phase 3 — Refinement:** richer analytics, additional entrainment methods,
  before/after measurement, polish and accessibility hardening.
- **Phase 4 (future):** objective measurement sources (EEG/HRV/tasks), optional
  cloud sync.

---

## 14. Success Criteria

- A user can identify which exact frequency (in 1 Hz steps) best produces a
  target state for them, by running and comparing logged sessions/programs.
- The example flow (5 Hz/10 min → 6 Hz/5 min) is fully buildable, runnable, and
  comparable in-app.
- Session parameters and outcomes are reliably captured and reviewable over time.
