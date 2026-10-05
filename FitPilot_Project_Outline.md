# Project Outline — FitPilot (AI Fitness Coach)

***

## 1. APP NAME

**FitPilot** — Your AI Fitness Coach that *sees*, *thinks* and *tracks*.

***

## 2. App Category

Health & Fitness

***

## 3. About the App

FitPilot is an Android application that acts as a **personal AI fitness coach**. Unlike traditional fitness apps where the user has to pick a course and follow it, FitPilot **decides what you should train today** and **watches you while you do it**.

The app combines three capabilities into one closed loop:

1. **It thinks (Daily Decision Engine):** Every morning, FitPilot recommends "what to train today" by combining the **weather & air quality** (e.g. rainy or polluted AQI → indoor/bodyweight session), the user's **schedule & preferred time**, their **Recovery Score** (from resting heart rate, sleep and past workouts), and their **training preference** (gym equipment vs. home bodyweight). A short onboarding questionnaire captures the user's **training scene (home vs gym) and equipment inventory** (dumbbells? treadmill? bodyweight-only?) so the engine can hard-filter the exercise library to what the user can actually do.
2. **It sees (Real-time AI Form Coach):** During strength training (e.g. bench press), the app uses the phone camera with an on-device pose-estimation model (TensorFlow Lite + MoveNet) to **count reps automatically and judge whether each rep is standard**, giving instant voice feedback (e.g. "Rep 5 — keep your elbows tucked").
3. **It tracks (Unified Training Log):** During running it records **heart rate** (BLE chest strap) and **distance** (GPS); during strength training it records **reps, sets and form quality**; it also logs **jump-rope counts** and **HIIT intervals**. All data is stored locally and feeds back into smarter future recommendations.

**Why there is a gap (Niche):** Existing apps cover parts of this idea but not the combination — Fitbod generates daily plans based on muscle recovery but ignores weather and gives no real-time form feedback; Keep has AI form scoring and follow-along courses but keeps running and strength as two separate systems, and its plans are "pick & play" rather than "decided for you"; Good Habits (open source) proves on-device rep counting works but does no recommendation at all. FitPilot is the first **fully local, explainable** coach that unifies **weather/AQI-aware daily decisions + a recovery score + real-time multi-exercise form feedback + unified multi-scenario training logs**.

***

## 4. Existing Similar Apps with Source Code

| App                             | App Details                                                                                                                                                                                                                                      | Source Code                                                                 |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------- |
| **Fitbod**                      | Generates personalized daily workout plans based on muscle recovery, equipment availability and progress history. Cloud subscription, **no weather dimension, no live form feedback**.                                                           | Not open source                                                             |
| **Keep**                        | Largest Chinese fitness platform: smart training plans, AI form scoring on follow-along videos, running + strength content — but the two scenarios are **separate systems**, plans are course-selection based, and AI scoring is server-side.    | Not open source                                                             |
| **Good Habits** *(chosen base)* | 100% native Android (Kotlin + Jetpack Compose + Room + MVVM), on-device TensorFlow Lite pose estimation with 90+ exercises, rep counting, streak tracking, CSV export. **Rep counting only — no recommendation, no weather, no voice coaching.** | [github.com/plana93/good-habits](https://github.com/plana93/good-habits)    |
| **HeartRunner**                 | Running app with BLE heart-rate strap, GPS track, pace stats and Chinese voice feedback; clean Compose + MVVM architecture. **Running only — no strength tracking, no recommendation.**                                                          | [github.com/igofreely/runner](https://github.com/igofreely/runner) (MIT)    |
| **wger**                        | Open fitness ecosystem: exercise database (500+), workout manager, free REST API. **Web/Flutter based, not native Android; no AI.**                                                                                                              | [github.com/wger-project/wger](https://github.com/wger-project/wger) (AGPL) |

**App chosen to build upon:** **Good Habits** (primary — pose-AI architecture, data layer and UI patterns are directly reusable), complemented by patterns from **HeartRunner** (BLE heart rate + GPS + TTS voice feedback). The exercise dataset is bundled locally (from Good Habits' built-in library and/or open exercise JSON data) so the app works **fully offline**.

***

## 5. Main New Added Features & Enhancements

Compared with the chosen base apps, FitPilot adds:

1. **User profile & equipment-inventory-driven recommendation engine** — a short onboarding questionnaire captures **training scene (home vs gym), equipment inventory (dumbbells / treadmill / bodyweight-only), training goal (fat loss / muscle gain / toning), weekly frequency and experience level**. Each exercise in the library carries tags (type: cardio vs strength; equipment; muscle group; difficulty), and the engine **hard-filters out exercises the user cannot do**, then ranks the rest. It also pulls **weather + air-quality (QWeather API)** so a polluted AQI automatically switches an outdoor run to an indoor plan.
2. **Recovery Score (0–100)** — a transparent score combining morning resting heart rate (BLE or manual), sleep & subjective fatigue check-in, and recent training load. It drives daily intensity and triggers overtraining warnings (e.g. "recovery low for 3 days → rest day").
3. **Real-time form-quality checking** — beyond counting reps, computes joint angles (shoulder/elbow/knee) to judge whether a rep is standard and gives corrective feedback; applied to bench press, squat and plank.
4. **Jump-rope auto counting** — pose-based detection of jump cycles for a cardio session, with on-screen count and voice milestones.
5. **Voice coaching feedback** — Android built-in TTS announces rep counts, correction hints, HIIT interval commands ("30s sprint / 15s rest") and a guided stretching cool-down (no cloud needed).
6. **Unified multi-scenario closed loop** — running (HR + distance), strength (reps + form), jump rope and HIIT data are unified in one Room database and feed future recommendations (recovery-aware intensity adjustment).
7. **Fully local & explainable AI** — pose estimation and the recommendation algorithm both run on-device, free, offline, and every decision is traceable.
8. *(Planned enhancement)* **LLM-assisted weekly summary** — a Chinese LLM API (e.g. DeepSeek) summarizes the week's training into natural language; non-critical path.

***

## 6. Functional Requirements (App Features)

| Ser | Existing Features (from chosen app)                     | Proposed Improvements / New Features                                                                                      |
| --- | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| 1   | Exercise library + manual workout logging (Good Habits) | **NEW:** Onboarding questionnaire (scene / equipment / goal / level) → tagged exercise library + hard-filter by equipment |
| 2   | Rep counting via on-device pose AI (Good Habits)        | **IMPROVED:** Form-quality judgement (joint angles) for bench press / squat / plank + jump-rope auto counting             |
| 3   | —                                                       | **NEW:** Running session: GPS distance, pace, BLE heart-rate recording with live chart                                    |
| 4   | —                                                       | **NEW:** Recovery Score (resting HR + sleep + fatigue check-in) with overtraining warning                                 |
| 5   | —                                                       | **NEW:** HIIT interval voice coach + guided stretching cool-down                                                          |
| 6   | —                                                       | **NEW:** Closed loop — past records automatically adjust next day's recommended intensity                                 |
| 7   | Streak / statistics (Good Habits)                       | **IMPROVED:** Unified charts & recovery trend visualization across all scenarios                                          |

***

## 7. Tool Stack

| Purpose            | Tool / API                       | Notes                                                                                                                                                                            |
| ------------------ | -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Weather & air data | **QWeather (和风天气)**              | Free monthly quota, **direct access in mainland China, no VPN**, provides weather icons, **air-quality index (AQI)** and sport/comfort indices used by the recommendation engine |
| Pose estimation    | **TensorFlow Lite + MoveNet**    | On-device ML, offline, free — the same approach proven by Good Habits                                                                                                            |
| Local database     | **Room** (SQLite)                | Exercises (with tags), workouts, running sessions, user profile & equipment inventory, recovery state                                                                            |
| UI                 | **Jetpack Compose + Material 3** | Modern declarative UI, dark mode                                                                                                                                                 |
| Network            | **Retrofit + OkHttp**            | QWeather API calls                                                                                                                                                               |
| Location           | **高德地图 SDK / osmdroid**          | GPS track for running (mainland-accessible)                                                                                                                                      |
| Heart rate         | **Android BLE API**              | Connect to standard BLE heart-rate chest strap                                                                                                                                   |
| Voice feedback     | **Android TextToSpeech**         | Built-in, offline, Chinese supported                                                                                                                                             |
| Architecture       | **MVVM + Repository**            | Follows Good Habits' clean structure                                                                                                                                             |
| Version control    | **Git + GitHub**                 | Team collaboration & commit history                                                                                                                                              |
| *(Planned)* LLM    | DeepSeek / 通义千问                  | Optional weekly summary only — non-critical path                                                                                                                                 |

***

## 8. Development Timeline

| Week | Work to be Completed                                                                                                                                                                                                                         | Comments        |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- |
| 6    | Scaffold Android project on the Good Habits base (Compose + Room + MVVM); design DB schema; import tagged exercise dataset (type / equipment / muscle / difficulty); onboarding questionnaire (scene / equipment / goal / frequency / level) | Sprint to Alpha |
| 7    | **Alpha submission** — recommendation engine v1 (equipment hard-filter + weighted scoring) + basic workout logging working                                                                                                                   | Milestone       |
| 8    | MoveNet pose integration: bench-press rep counting + joint-angle form judgement                                                                                                                                                              | Sprint to Beta  |
| 9    | TTS voice feedback; running session: GPS distance & pace (osmdroid/高德) + BLE heart-rate strap; live HR chart                                                                                                                                 | <br />          |
| 10   | **Beta submission** — real-time form feedback + running/heart-rate loop complete                                                                                                                                                             | Milestone       |
| 11   | Recovery Score (sleep / fatigue check-in + resting HR) → closed-loop intensity adjustment                                                                                                                                                    | <br />          |
| 12   | QWeather AQI integration; unified history & recovery-trend charts; UI polish / dark mode                                                                                                                                                     | <br />          |
| 13   | Edge cases (no network / no BLE / no camera permission); documentation; demo script; *(if time)* jump-rope / HIIT / stretching                                                                                                               | Optional extras |
| 14   | **Final submission** — video + report + live demo                                                                                                                                                                                            | Milestone       |

***

## 9. Key Contributions (Summary)

1. A **weather + AQI-aware daily decision engine** that existing fitness apps lack (polluted AQI → indoor plan).
2. A **Recovery Score** that turns raw logs into a transparent "how fresh am I" number, driving daily intensity and rest-day warnings.
3. **Real-time form feedback with voice coaching** across strength (bench press / squat / plank), jump rope and HIIT — fully on-device and explainable.
4. A **unified multi-scenario training log** (running + strength + HIIT) that closes the data loop into smarter recommendations.

