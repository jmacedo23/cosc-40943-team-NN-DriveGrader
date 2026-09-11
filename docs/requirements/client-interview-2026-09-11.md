# Drive Grader — Meeting Transcript & Project Notes

*CS Senior Project — Client Meeting*

### 1. The Problem Drive Grader Solves

**Client:** Drive Grader isn't complicated. It's built around a common situation: a parent teaching their own teenager to drive. Parent-taught driver's education is very popular in Texas because it's cheap and doesn't require any instructor qualifications — but it also tends to produce poorly trained drivers. Parents usually don't know what to look for or how to teach. They just get in the car and say things like "go that way," "don't hit that cone," or "don't hit the car behind you." They have no real tools or structure to train their kid well enough to pass the road test. Drive Grader is meant to give them that structure.

### 2. Demo Walkthrough: Starting a Drive

**Client:** To start a session, you create a new drive — for example, "Basic Skills" for "Test Driver." Most settings can be left at their defaults. Since I'm not actually in a car right now, I selected a simulated GPS and a simulated OBD2 device.

> OBD2 — "on-board diagnostics" — is the standard diagnostic port built into every modern car, normally used to read engine and vehicle data.

**Client:** Once you hit "begin drive," the app tracks the phone's GPS (speed, route) and accelerometer (hard braking, hard turns, G-forces) in real time. As the instructor or parent, you have a list of about ten grading topics — following distance, smooth braking, failure to control speed, lane discipline, situational awareness, and others — and you tap the relevant one whenever the student makes a mistake, logging it as an infraction. When the drive ends, the app shows the full route that was driven along with every infraction that was logged and when.

### 3. OBD2 and Vehicle Data Integration

**Client:** The plan is to see whether an actual OBD2 device can feed live data into the app. OBD2 units are small, universal, and plug into any car; they connect to a phone over Bluetooth. Depending on the vehicle, some OBD2 devices can also read the CAN bus signal, which reports things like turn-signal use and brake-light activation — not just accelerator/engine data. If the team can capture brake and turn-signal use, that's valuable extra metadata to combine with GPS and accelerometer data, and to track a driver's trends over time.

Open questions: whether a standard OBD2 unit actually exposes turn-signal/brake data or whether another approach is needed, and how (or whether) this works with electric vehicles, which the client noted may behave differently.

### 4. Texas Driver's-Ed Requirements & Road Test Criteria

**Client:** Texas requires teen drivers to log 7 hours of behind-the-wheel instruction, 7 hours of observation, and 30 hours of general logged driving — 44 hours total — before testing. In practice nobody verifies this, but Drive Grader could track it automatically. The app also teaches parents what examiners actually check on the real road test: things like stopping at the stop line (not past it, not short of it) and "approach to corner" — looking both directions at intersections that don't have stop signs. These are the same items graded on the state's official form, the DL-40.

### 5. The DL-40 Digital Grading Feature

**Client:** This is actually the reason Drive Grader started. Examiners currently grade road tests on paper using the DL-40, which lists checklist items in one fixed order. But every driving route is different — routes are built to include a required set of maneuvers (three left turns, three right turns, two stop signs, two traffic lights, two approach-to-corners, a parallel park, a reverse, etc.), not to match the DL-40's printed order. So examiners end up hunting up and down the sheet to find the matching item as each maneuver happens.

**Client:** We asked the Texas Department of Public Safety whether we could grade electronically and print the result afterward, and they approved it. So in the app, you can reorder the DL-40 checklist to match your specific route, select the driver and parent/guardian (with signatures), and then step through the drive tapping each maneuver as it happens — parking, merge, lane change, approach-to-corner, traffic signal, traffic sign, left turns, right turns, backing, and so on. At the end, the app prints a completed, signed DL-40 grade sheet.

**Student:** For Drive Grader, is a parent supposed to input the grading data themselves?

**Client:** Yes — it just gives them a tool. Otherwise they're driving around telling their kid "go that way, don't hit anything," with no idea what to actually grade them on or how. Outside of a formal road test, a parent can use the same app for a normal practice drive and log infractions the same way, building a history of improvement over time.

### 6. Technical Stack

**Client:** The current build uses a Quasar (Vue-based) front end with a Node.js API back end, wrapped as a Progressive Web App (PWA), built using Cursor — an AI-assisted code editor similar to VS Code. A PWA installs like a regular app and hides the browser's navigation bar, but has limited access to a phone's internals. Wrapping it further with Capacitor turns it into a true native-style app with full iOS/Android access, and would allow publishing to the App Store or Play Store. The team will need to determine whether a PWA gives enough device access for OBD2/Bluetooth, or whether the project needs to move to a native Capacitor build.

**Client:** Mapping currently uses OpenStreetMap, an open-source map service; in an actual car it would track the phone's live GPS location and draw the real route driven. There's also an existing SaaS-style admin panel with organizations, drive plans (drive time, road-test practice, etc.), maneuvers and score criteria, session times, and integration settings (for example, a future integration with a reservation system).

### 7. Alternative Project Option (Considered, Not Chosen)

The client also offered a second possible project: a service-industry scheduling/dispatch application, similar to Housecall Pro, used by trade businesses (HVAC, electrical, plumbing, glass repair, etc.) to schedule jobs, dispatch technicians, and text customers a live tracking link when a technician is on the way. The client had experimented with using AI to quickly rebuild a Housecall-Pro-style app as a demo. The group ultimately chose to move forward with Drive Grader instead.

### 8. Class Logistics, Access & Constraints

- The team will get access to the client's GitHub organization; GitHub Actions will auto-deploy pushed code to a staging environment (the same one currently running on the client's phone).
- Backend database: MySQL 8.
- AI integration is optional, not required — the client doesn't see an obvious use case for AI inside Drive Grader itself, but is open to it if the team comes up with a good reason.
- The team will be given a team account for AI coding tools (Cursor, Claude, etc.) to use during development.
- Class deliverables: a project proposal/plan is due to the professor soon, with periodic check-ins/benchmarks after that. The goal is a working MVP by the end of the semester, with final project handoff around January.
- Four OBD2 devices have already been purchased (roughly $20–$30 each on Amazon) for the team to test with; none had been tested yet as of this meeting.

### 9. Team Q&A Highlights

**Student:** Are there any design preferences for the app?

**Client:** No specific design preferences — whatever the team comes up with is fine. The priority is that it's intuitive: something you pick up and immediately understand, not necessarily polished.

**Student:** Are there specific training requirements — hours or skills — that need to be tracked?

**Client:** Yes: 30 hours of general logged driving, plus 7 hours behind-the-wheel instruction and 7 hours of observation (44 total). This could be modeled as configurable variables — required hours per category — with the app tracking progress (e.g., percentage complete) across each category.

**Student:** Are the OBD2 devices Bluetooth?

**Client:** Yes, plug-and-pair over Bluetooth — no separate app or data-sharing service required. The idea for using CAN-bus data (turn signal, brake, RPM, etc.) was to see if it's possible to know when a student used their turn signal, hit the brake, or how hard they accelerated, then correlate that with the GPS map data. It's untested, and it's unclear yet how well it will work, especially on electric vehicles.

### 10. Other Discussion (Not Directly Project-Related)

For completeness, the meeting also covered several topics unrelated to Drive Grader itself:

- General discussion of AI's impact on the software industry — concerns about AI reducing the value of custom software and shrinking demand for entry-level programming jobs; mention of a former student employee who left for a job elsewhere after a few months.
- The client's other AI product, an all-in-one business tool used internally for email monitoring/response, chat, and similar tasks, including a demo integration with a weather API and discussion of AI cost/token usage and running local AI models.
- A broader tangent on AI's long-term societal effects (referencing the film *Idiocracy*) and concerns about AI discouraging independent/critical thinking.
- A friend's DARPA-funded drone startup working on jet-propulsion-driven heavy-lift drones.
- Personal background conversation — the client's time in the U.S. Navy, growing up in Fort Worth, and various team members' unrelated details (ROTC, internships, dorm life, certifications).

---

## Project Notes — Drive Grader Summary

*A concise, project-only summary for reference — tangents and off-topic discussion excluded.*

### What It Is

A mobile/web app that helps parents who are informally teaching their teen to drive (very common in Texas, since parent-taught driver's ed is cheap and unregulated) know what to look for and how to grade their kid's driving — solving the core problem that most parents have no structure or training tools to work from.

### Core Functionality

- **Drive sessions:** start a session with a driver profile, GPS tracking, and an optional connected OBD2 device.
- **Live tracking:** speed and route via GPS; hard braking, hard turns, and G-forces via accelerometer.
- **~10 grading categories:** e.g. following distance, smooth braking, speed control, lane discipline, situational awareness — tapped by the instructor/parent in real time to log infractions.
- **End-of-drive summary:** full route driven plus a timestamped list of infractions.
- **Digital DL-40 grading mode:** replicates Texas's official road-test grade sheet, but lets the checklist be reordered to match the actual test route instead of a fixed printed order. Captures driver and parent/guardian info plus signatures, and prints a completed DL-40 at the end. Confirmed allowed by Texas DPS.
- **Hour logging:** tracks progress toward the required 30 hours general driving + 7 hours behind-the-wheel + 7 hours observation (44 total).

### Hardware Integration (OBD2)

- Universal, Bluetooth OBD2 device plugs into any car to pull vehicle data — no companion app or data-sharing service needed.
- Investigating whether OBD2/CAN bus data can also expose turn-signal and brake-light use (not just speed/engine data) for richer grading input.
- 4 OBD2 devices already purchased (~$20–$30 each) for testing; untested as of this meeting.
- Open question: how (or whether) this works on electric vehicles.

### Current Tech Stack

- Front end: Quasar (Vue-based framework).
- Back end: Node.js API with MySQL 8.
- Currently a Progressive Web App (PWA); may need a Capacitor wrapper for full native iOS/Android access if PWA can't reach the OBD2/Bluetooth APIs needed.
- Mapping via OpenStreetMap.
- Existing SaaS admin panel: organizations, drive plans, maneuvers/score criteria, session times, integration settings.
- Dev environment: Cursor (AI-assisted IDE); team gets GitHub org access (GitHub Actions auto-deploys to staging) and a team AI-tool account.

### Client Constraints & Preferences

- No strong design/UI preference — priority is that the app is intuitive, not necessarily polished.
- AI integration is optional, not required — open to good ideas but no specific use case requested.
- Backend must use MySQL 8.
- MVP expected by end of semester; final project handoff around January.

### Open Questions for the Team

1. Can a standard OBD2/CAN device expose turn-signal and brake-light data?
2. Does OBD2 integration work the same way on electric vehicles?
3. Is a PWA sufficient for the device access needed, or is a native (Capacitor) build required?
4. How should the 30/7/7-hour requirement be modeled — e.g., configurable target hours per category with progress tracking?
