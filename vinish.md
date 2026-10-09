StoreFlow — Complete Project Blueprint
1. Project identity
Project name
StoreFlow
Positioning
Waze for the inside of a store
Core pitch line
We don't track customers. We track the store.

Main tagline
We don't just show where customers are. We show where the store makes them stop, where traffic breaks down, where space is being ignored, and what to change.

Core operating loop
Observe → Diagnose → Prescribe → Verify

This four-step loop is the central idea behind the entire product.
2. The problem
Physical retailers already have security cameras, but those cameras are primarily used for security and post-event observation.
The store manager still has to manually answer questions such as:
- Which areas attract the most traffic?
- Which sections are being ignored?
- Where does movement slow down?
- Which aisle repeatedly becomes congested?
- Are promotional displays helping or obstructing traffic?
- Is the layout making customers bypass certain sections?
- Did a recent layout change actually improve the store?
Traditional CCTV gives video.
Traditional analytics may give a heatmap.
But neither necessarily gives the manager a decision.
The fundamental problem
Existing retail cameras capture movement, but the store lacks a system that converts that movement into actionable spatial intelligence.

3. The insight behind StoreFlow
The key insight is:
We don't need to know who the people are. We need to understand how the physical store behaves when people move through it.

So StoreFlow treats anonymous movement as a sensor for the store.
We aren't building:
"A customer tracking system."

We are building:
"A store behavior and layout intelligence system."

The system measures:
- Traffic
- Flow
- Density
- Dwell
- Speed
- Zone transitions
- Congestion
- Underused space
- Repeated movement patterns
- Effect of layout changes
4. The StoreFlow mental model
The easiest way to understand the product is to imagine the store as a road network.
Retail store	Road network
Customer movement	Vehicles
Aisles	Roads
Store sections	Destinations
Junctions	Intersections
Congestion	Traffic jam
Dead zone	Unused road
Dwell	Stopping time
Journey	Route
Store layout	Road network
StoreFlow	Waze-like traffic intelligence


This creates the Store Graph.
5. Store Graph
The physical store is represented digitally as a graph.
                     Electronics
                          ●
                          │
                          │
Entrance ●────●───────────●────────● Checkout
          A1   │          │
               │          │
               ●          ●
            Grocery      Promo

Graph components
Nodes
- Store sections
- Aisles
- Checkout area
- Entrance/exit
- Promotional zones
Edges
- Corridors
- Aisle connections
- Passageways
Graph events
- Traffic passing through a zone
- Entry into a zone
- Exit from a zone
- Movement between zones
- Build-up at a node
- Slowdown on an edge
Now the system can understand the store as a network of physical movement, rather than just a video feed.
6. What StoreFlow actually does
The full system is:
Existing CCTV
      ↓
Video Processing
      ↓
Person Detection
      ↓
Anonymous Object Tracking
      ↓
Ground-Plane Mapping
      ↓
Zone Mapping
      ↓
Store Graph
      ↓
Spatial Metrics
      ↓
Behavior / Flow Analysis
      ↓
Problem Detection
      ↓
Recommendation Engine
      ↓
Manager Dashboard
      ↓
Layout Experiment
      ↓
Before / After Verification

This is the entire product in one pipeline.
7. Technical architecture
High-level architecture
                 ┌─────────────────────┐
                 │ Existing CCTV       │
                 │ RTSP / Recorded      │
                 │ Video               │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Video Ingestion     │
                 │ FFmpeg / OpenCV     │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Person Detection    │
                 │ YOLO                │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Object Tracking     │
                 │ ByteTrack /         │
                 │ BoT-SORT            │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Ground Plane        │
                 │ Mapping /           │
                 │ Homography          │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Store Zone Mapper   │
                 │ Floor Plan +       │
                 │ Polygons           │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Store Graph Engine  │
                 └──────────┬──────────┘
                            │
             ┌──────────────┼───────────────┐
             ▼              ▼               ▼
        Traffic Engine   Dwell Engine   Flow Engine
             │              │               │
             └──────────────┼───────────────┘
                            ▼
                 ┌─────────────────────┐
                 │ Spatial Intelligence│
                 │ Engine              │
                 └──────────┬──────────┘
                            │
          ┌─────────────────┼─────────────────┐
          ▼                 ▼                 ▼
     Bottleneck         Dead Zone       Flow Drop-off
     Detection          Detection        Detection
          │                 │                 │
          └─────────────────┼─────────────────┘
                            ▼
                 ┌─────────────────────┐
                 │ Recommendation      │
                 │ Engine              │
                 └──────────┬──────────┘
                            ▼
                 ┌─────────────────────┐
                 │ Manager Dashboard   │
                 │ React               │
                 └──────────┬──────────┘
                            ▼
                 ┌─────────────────────┐
                 │ Layout Experiment   │
                 │ Before / After      │
                 └─────────────────────┘

8. Step 1 — CCTV ingestion
StoreFlow should work with standard existing security cameras.
Input
Potentially:
RTSP Camera Feed

or:
Recorded MP4

for the competition/demo.
Processing
Use:
- FFmpeg
- OpenCV
- Python
The pipeline doesn't need to process every frame.
For example:
Camera
30 FPS
  ↓
Frame sampling
5–10 FPS
  ↓
CV processing

This dramatically reduces compute requirements while still giving sufficiently accurate movement information.
9. Step 2 — Person detection
A computer-vision detector identifies people in each frame.
Example:
Frame
   ↓
YOLO
   ↓
Person bounding boxes

Output:
Person 1 → x1,y1,x2,y2
Person 2 → x1,y1,x2,y2
Person 3 → x1,y1,x2,y2

The system doesn't need identity.
It only needs the location of movement.
10. Step 3 — Anonymous tracking
A tracker associates detections across frames.
For example:
Frame 100 → Person
Frame 101 → Person
Frame 102 → Person
Frame 103 → Person

The tracker assigns:
Track_ID = 127

and continues following that anonymous object.
Potential technologies:
- ByteTrack
- BoT-SORT
Important principle
The track ID is not a person identity.
It exists only to reconstruct movement temporarily.
The system stores spatial observations such as:
track_id
timestamp
x
y
zone
speed
direction

rather than identity information.
11. Step 4 — Convert camera coordinates into store coordinates
This is one of the important technical pieces.
A camera sees:
pixels

but StoreFlow needs:
physical store position

For example:
Camera frame

(100,300)
    ↓
Store coordinate

(4.2m, 12.7m)

This can be implemented using a homography transformation for planar floor mapping.
Why this matters
Without ground-plane mapping:
1 pixel of movement doesn't represent a meaningful physical distance.

With mapping:
speed can be estimated in meters/second.

This allows meaningful measurements such as:
- Walking velocity
- Distance travelled
- Zone occupancy
- Movement direction
12. Step 5 — Store map configuration
Before analytics begin, the manager/admin creates a digital store map.
Example:
┌────────────────────────────────────────┐
│ Entrance                               │
│   ↓                                    │
│ ┌────┐ ┌────┐ ┌────┐ ┌─────────────┐ │
│ │ A1 │ │ A2 │ │ A3 │ │ Electronics │ │
│ └────┘ └────┘ └────┘ └─────────────┘ │
│                                        │
│ ┌────────┐ ┌──────────────┐            │
│ │Grocery │ │ Promo Display│            │
│ └────────┘ └──────────────┘            │
│                              Checkout   │
└────────────────────────────────────────┘

Each region gets a zone polygon.
Example:
Zone ID: Z04
Name: Electronics
Polygon: [(...), (...), (...), (...)]

The manager can also configure:
- Fixture locations
- Promotional displays
- Aisles
- Checkout
- Entrance
- Exit
- Important corridors
This is also where the system gets context for recommendation generation.
13. Step 6 — Map movement into zones
Now every anonymous track becomes a sequence of store states.
Example:
Track 127

10:32:11 → Entrance
10:32:19 → Aisle 1
10:32:44 → Grocery
10:34:02 → Promo
10:35:51 → Electronics
10:39:10 → Checkout
10:40:02 → Exit

This produces a journey:
Entrance
   ↓
Aisle 1
   ↓
Grocery
   ↓
Promo
   ↓
Electronics
   ↓
Checkout
   ↓
Exit

But the system doesn't need to preserve the identity of Track 127 long-term.
It can aggregate the journeys into:
Most common store routes

14. Core metrics
This is where video becomes intelligence.
A. Traffic
How much movement passes through a zone?
Example:
Electronics
1,240 zone visits

B. Density
How many anonymous tracked objects occupy the area at a given time?
Example:
Aisle 4
Current density: 82%

C. Dwell
How long does activity remain within a zone?
Important distinction:
Dwell is not simply "how long a track exists".
The system measures the amount of time an anonymous track remains within the zone, with rules to avoid counting brief transit.
Example:
Electronics
Average dwell = 4.1 minutes

D. Velocity
Measure movement speed through a zone.
Example:
Normal velocity = 0.9 m/s
Observed = 0.35 m/s

A significant recurring decrease can indicate friction.
E. Zone transitions
Count:
Aisle 1 → Grocery
Grocery → Promo
Promo → Electronics

This creates the Store Graph traffic.
F. Route frequency
Find commonly occurring sequences.
Example:
Entrance → A1 → Grocery → Checkout
              38%

Entrance → A1 → Electronics → Checkout
              22%

15. Establishing a baseline
This is very important for making the system intelligent rather than arbitrary.
A zone shouldn't be judged by a fixed universal threshold.
Aisle 1 might naturally be busy.
Checkout is expected to have long dwell.
Therefore StoreFlow learns the normal behavior of each zone.
For every zone and time period, calculate historical baselines such as:
Expected density
Expected dwell
Expected velocity
Expected traffic
Expected queue level

Example:
Aisle 4 — 7 PM baseline

Expected density: 45%
Expected speed: 0.82 m/s
Expected dwell: 1.2 min

Observed today:
Density: 82%
Speed: 0.31 m/s
Dwell: 2.7 min

Now the system can identify abnormal spatial behavior.
16. Signature feature — Store Friction Score
Every zone gets a score from 0–100.
Conceptually:
Friction =
    abnormal density
  + abnormal dwell
  + abnormal slowdown
  + abnormal congestion
  + repeated occurrence

The exact weights can be tuned experimentally.
For example:
Density deviation      30%
Velocity deviation     30%
Dwell deviation        20%
Queue/congestion       15%
Recurrence              5%

Then:
0–20    🟢 Healthy
20–40   🟡 Watch
40–70   🟠 Friction
70–100  🔴 Critical

The pitch
One number per zone that tells the manager how strongly that area is disrupting the store's normal flow.

17. Bottleneck detection
A bottleneck isn't just "a crowded area."
The system looks for a pattern.
Candidate bottleneck
High density
      +
Reduced velocity
      +
Elevated dwell
      +
Repeated at similar times

Example:
Aisle 4

Density       +52%
Velocity      -61%
Dwell         +74%
Occurrence    6 of last 7 days

Result:
🔴 Recurring Bottleneck

The system can tell the manager:
- Where it is
- When it occurs
- How severe it is
- How frequently it occurs
18. Dead-zone detection
A dead zone should not simply mean:
"Very few people."

The system compares the zone against the store's normal movement network.
Example:
Aisle 1     1,240 visits
Aisle 2     1,080 visits
Aisle 3       950 visits
Aisle 4        89 visits
Aisle 5     1,110 visits

Then analyse:
- Traffic volume
- Number of journeys entering zone
- Connections to adjacent zones
- Time spent there
- Whether common routes bypass it
Result:
🕳️ Aisle 4 is consistently bypassed by normal store journeys.

19. Flow drop-off detection
This replaces an unsafe claim like "conversion detection".
The system can identify:
High activity in one area that fails to propagate toward the next connected area.

Example:
Promo Display
      ↓
High traffic
      ↓
High dwell
      ↓
Low onward movement
      ↓
FLOW DROP-OFF

This means:
"The zone attracts activity but does not efficiently route movement onward."

Without POS data, StoreFlow should not claim that this equals lost sales or conversion.
20. Why does a problem happen?
This is where you need to be technically honest.
Computer vision can detect:
"Traffic slows here."

But it cannot automatically know with certainty:
"The red display causes the slowdown."

So StoreFlow uses context supplied through the store map.
The manager can configure:
Fixture
Promotional Display
Checkout
Aisle
Entrance

Then the system can produce cause hints, such as:
"Congestion overlaps the configured promotional display area."

or:
"Low traffic zone is isolated from the main movement corridor."

This is much more defensible than pretending CV has causal understanding.
21. Recommendation engine
Now StoreFlow moves from:
Analytics

to:
Decision support

Recommendations are initially rule-based.
Examples:
Bottleneck
Detected:
High density + slowdown near fixture

Recommendation:
Review promotional fixture placement or corridor width during peak hours.

Dead zone
Detected:
Low traffic + repeated bypass

Recommendation:
Consider relocating high-interest merchandise or a promotional marker closer to the main flow.

Flow drop-off
Detected:
High dwell but weak onward movement

Recommendation:
Test a different fixture orientation or promotional placement and compare post-change flow.

Notice the language:
"Test", "review", "consider"

rather than pretending the AI knows the one correct solution.
22. Manager dashboard structure
The manager shouldn't be given a giant CV analytics screen.
The UI should answer:
What is happening?

Where?

Why?

What should I do?

Did it work?

Dashboard 1 — Store Overview
Purpose
Daily health check.
STORE FLOW SCORE
82 / 100 🟢

Traffic
1,284

Active Issues
4

Critical
1

Dead Zones
1

Then:
Live store map
🟢 Healthy
🟡 Watch
🟠 Friction
🔴 Critical

And a summary:
2 Bottlenecks
1 Dead Zone
1 Flow Drop-off
8 Healthy Zones

Dashboard 2 — Store Map
Interactive floor map.
The manager can switch between:
Traffic
Where movement is concentrated.
Dwell
Where activity remains longer.
Flow
How movement travels through the store.
Friction
Where store flow is disrupted.
This is essentially the Waze-like view of the store.
Dashboard 3 — Issues
This is the manager's action queue.
Example:
🔴 CRITICAL

Aisle 4 Bottleneck
Friction Score: 82

Recurring 6:30–8 PM

[Investigate]


🟠 FRICTION

Promo Zone
Friction Score: 67

High dwell / low onward flow

[Investigate]


🟡 DEAD ZONE

Aisle 7
Friction Score: 71

Repeated route bypass

[Investigate]

The manager doesn't need to search through graphs.
StoreFlow prioritizes the problems.
Dashboard 4 — Zone Analytics
Clicking a zone opens:
AISLE 4

Friction
82 🔴

Traffic       +43%
Density       +52%
Dwell         +74%
Velocity      -61%

Peak:
6:30–8:00 PM

Recurrence:
6 / 7 days

Then charts:
- Hourly traffic
- Hourly dwell
- Speed
- Density
- Zone transitions
- Historical trend
Dashboard 5 — Layout Experiments
This is one of the most important features.
Manager creates:
Experiment #12

Change:
Move Promotional Display

Start:
12 Oct

Baseline:
Previous 7 days

StoreFlow measures:
Before vs After
Metric	Before	After	Change
Store Flow Score	68	86	+26%
Avg velocity	0.71	0.91 m/s	+28%
Bottlenecks	3	1	-67%
Aisle 4 dwell	4.2 min	2.8 min	-33%
Dead zones	2	1	-50%


Then:
🟢 Layout change improved store flow.

This creates the product's complete feedback loop.
Dashboard 6 — Trends
For longer-term management:
30-DAY STORE FLOW

Week 1   68
Week 2   71
Week 3   79
Week 4   86

Manager can compare:
- Today vs yesterday
- This week vs last week
- Weekday vs weekend
- Morning vs evening
- Before vs after changes
23. Ultimate user experience
The manager's workflow should feel like this.
Morning
StoreFlow says:
Your store has 2 critical issues.

Manager clicks
Aisle 4 is producing recurring peak-hour congestion.

StoreFlow explains
Density +52%, velocity -61%, dwell +74%.

StoreFlow recommends
Review promotional display placement around this corridor.

Manager changes layout
Display moved.
One week later
StoreFlow says:
Flow improved by 26%.

That is the actual value proposition.
24. Privacy design
The project should explicitly follow:
We don't identify people. We measure space.

Required data
Temporary track ID
Timestamp
Position
Zone
Speed
Direction

Explicitly not required
Face
Name
Phone
Email
Identity
Demographics
Personal profile

Recommended implementation
For the competition/demo:
- Process video locally where practical
- Store derived spatial metrics
- Aggregate historical analytics
- Minimize retention of raw footage
- Don't build face recognition
This makes the privacy architecture part of the product rather than an afterthought.
25. Recommended technology stack
Computer Vision
Python
OpenCV
YOLO
ByteTrack / BoT-SORT
NumPy

Spatial processing
Homography
Polygon zone mapping
Trajectory processing
Geometric calculations

Backend
Django
Django REST Framework
PostgreSQL
Redis
WebSockets

Frontend
React
Tailwind CSS
Recharts
SVG / Canvas

Infrastructure
Docker
Nginx
AWS / local GPU machine
RTSP / FFmpeg

For the first demo, a recorded CCTV video is enough. Live RTSP can be added after the analytics pipeline works reliably.
26. Suggested backend architecture
                 ┌──────────────────┐
                 │ Camera Ingestion │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │ CV Worker        │
                 │ YOLO + Tracker   │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │ Spatial Worker   │
                 │ Homography       │
                 │ Zone Mapping     │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │ Analytics Engine │
                 │ Traffic/Dwell    │
                 │ Flow/Friction    │
                 └────────┬─────────┘
                          │
                 ┌────────┴────────┐
                 ▼                 ▼
           PostgreSQL           Redis
           historical          realtime
             data               state
                 │                 │
                 └────────┬────────┘
                          ▼
                 Django REST API
                          │
                          ▼
                    React Dashboard

27. Suggested database structure
A practical initial schema:
Store
id
name
floor_plan

Camera
id
store_id
rtsp_url
position
calibration

Zone
id
store_id
name
polygon
zone_type

Fixture
id
zone_id
type
position

TrackObservation
id
camera_id
timestamp
track_id
x
y
zone_id
speed
direction

ZoneMetric
zone_id
timestamp
traffic
density
avg_dwell
avg_speed

Issue
zone_id
type
severity
score
detected_at

Experiment
id
store_id
description
start_date
baseline_period
evaluation_period

ExperimentMetric
experiment_id
metric
before_value
after_value
change_percent

You don't need a massive database for the initial prototype.
28. Real-time versus historical processing
Not every component needs to be real-time.
Real-time
Useful for:
- Current traffic
- Current density
- Current congestion
- Live store map
- Alerts
Near-real-time / batch
Useful for:
- Dwell calculation
- Journey patterns
- Dead-zone detection
- Historical trends
- Friction scores
- Experiment evaluation
This distinction keeps the system computationally practical.
29. MVP
Don't try to build everything at once.
Phase 1 — Core CV
Build:
Video
 ↓
YOLO
 ↓
ByteTrack
 ↓
Trajectory

Success criterion:
Accurate anonymous movement tracking.

Phase 2 — Store mapping
Add:
Homography
+
Floor plan
+
Zone polygons

Success criterion:
Every movement can be assigned to a physical store zone.

Phase 3 — Metrics
Implement:
- Traffic
- Dwell
- Density
- Speed
- Zone transitions
Success criterion:
Store behavior can be quantified.

Phase 4 — Intelligence
Implement:
- Bottleneck detection
- Dead-zone detection
- Flow drop-off
- Friction Score
Success criterion:
System automatically identifies store problems.

Phase 5 — Dashboard
Build:
- Overview
- Store map
- Issues
- Zone analytics
Success criterion:
Manager can understand the store without looking at raw video.

Phase 6 — Recommendations
Implement initial rule-based recommendations.
Example:
IF
density high
AND
velocity low
AND
fixture nearby

THEN

flag possible fixture-related friction

Success criterion:
System provides actionable next steps.

Phase 7 — Experiments
Add:
- Baseline snapshots
- Experiment start
- After-period comparison
- Improvement report
Success criterion:
Manager can prove whether a layout change worked.

30. Advanced version
After the MVP works, you can add:
Multi-camera tracking
Combine overlapping cameras into a continuous store movement model.
Queue intelligence
Detect queue formation and approximate queue length.
Predictive congestion
Predict:
"Aisle 4 is likely to become congested in the next 15 minutes."

POS integration
This is where you can eventually connect:
Spatial behavior
      +
POS sales
      ↓
Actual conversion / revenue analysis

But do not make POS integration part of the core MVP.
Multi-store comparison
Store A → Flow Score 84
Store B → Flow Score 71
Store C → Flow Score 89

Layout simulation
Potential future feature:
"What happens if this display moves here?"

That would require a more advanced spatial simulation model.
31. What makes StoreFlow different
Typical retail CV solution
Camera
 ↓
Person detection
 ↓
Heatmap
 ↓
Dashboard

StoreFlow
Camera
 ↓
Anonymous movement
 ↓
Spatial model
 ↓
Store Graph
 ↓
Traffic + Dwell + Flow
 ↓
Friction Score
 ↓
Automatic diagnosis
 ↓
Recommended intervention
 ↓
Layout experiment
 ↓
Before / After verification

The difference is closing the decision loop.
32. The four layers of intelligence
This is a very useful framework for your presentation.
Layer 1 — SEE
What is happening?
Computer vision.
People
Positions
Movement

Layer 2 — UNDERSTAND
What does that mean for the store?
Spatial analytics.
Traffic
Dwell
Density
Speed
Routes

Layer 3 — DECIDE
What needs attention?
Store intelligence.
Bottleneck
Dead zone
Flow drop-off
Friction

Layer 4 — PROVE
Did the intervention work?
Experiment engine.
Before
vs
After

This is perhaps the strongest product architecture for your pitch.
33. The ultimate manager value
The manager gets much more than a heatmap.
They get:
Visibility
"How is my store behaving?"

Diagnosis
"Where is the problem?"

Explanation
"What spatial condition is associated with it?"

Prioritization
"Which problem should I solve first?"

Action support
"What layout change should I test?"

Verification
"Did that change actually improve the store?"

This is the real business value.
34. What the manager ultimately receives
At the end, StoreFlow should boil everything down into a few decision-oriented outputs.
Daily Store Report
STORE FLOW SCORE
82 / 100

Today's findings:
🔴 Aisle 4 bottleneck
🟠 Promo-zone flow drop-off
🟡 Aisle 7 dead zone

Recommended action:
Review Promo Display placement

Yesterday:
76

Improvement:
+8%

Zone report
AISLE 4

Friction Score: 82

Problem:
Peak-hour congestion

Peak:
6:30–8 PM

Observed:
Density +52%
Speed -61%
Dwell +74%

Action:
Review fixture placement

Experiment report
EXPERIMENT #12

Moved promotional display

RESULT:
Store Flow     +26%
Bottlenecks    -67%
Velocity       +28%
Dead Zones     -50%

VERDICT:
🟢 POSITIVE

This is what transforms the product from an analytics dashboard into a decision-support platform.
35. Main pitch narrative
For the competition, the story should flow like this:
Problem
Retail stores already have cameras, but they don't have spatial intelligence.

Insight
A store's physical layout can be measured through anonymous movement.

Solution
StoreFlow converts CCTV into a live model of store flow.

Differentiation
We go beyond heatmaps using the Store Graph and Friction Score.

Intelligence
We automatically identify bottlenecks, dead zones and flow drop-offs.

Action
We recommend what the manager should investigate or change.

Verification
We compare the store before and after the intervention.

Outcome
The manager no longer guesses whether a layout works—they can measure it.

36. Final pitch
Here is the version I'd use as the core project pitch:
Every retail store already has cameras. But those cameras mainly tell us what happened—they don't tell us how the store itself is performing.
StoreFlow turns existing CCTV into a spatial intelligence system for the store.
We don't track customers. We track the store.
Anonymous movement is converted into a digital Store Graph that measures traffic, dwell, density, speed and flow across every section.
StoreFlow then assigns each area a Friction Score and automatically identifies bottlenecks, dead zones and flow drop-offs.
But we don't stop at diagnosis.
We recommend what the manager should investigate, let them test a layout change, and measure whether the store actually improved.
Observe. Diagnose. Prescribe. Verify.
StoreFlow turns a security camera into a sensor for better store decisions.

37. One-line explanation for judges
If someone asks:
"What exactly is your project?"

Say:
"StoreFlow is a privacy-preserving retail spatial intelligence platform that uses existing CCTV to understand store-wide movement, automatically detect layout friction, and verify whether layout changes improve traffic flow."

38. The core product in one diagram
                EXISTING STORE CAMERAS
                         │
                         ▼
                  ANONYMOUS MOTION
                         │
                         ▼
                  STORE SPATIAL MAP
                         │
             ┌───────────┼───────────┐
             ▼           ▼           ▼
          Traffic      Dwell       Flow
             │           │           │
             └───────────┼───────────┘
                         ▼
                  STORE GRAPH
                         │
                         ▼
                 FRICTION ENGINE
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
          BOTTLENECK  DEAD ZONE  FLOW DROP
              │          │          │
              └──────────┼──────────┘
                         ▼
                  ACTION ENGINE
                         │
                         ▼
                  MANAGER DASHBOARD
                         │
                         ▼
                  LAYOUT CHANGE
                         │
                         ▼
                    EXPERIMENT
                         │
                         ▼
                  BEFORE vs AFTER
                         │
                         ▼
                STORE IMPROVEMENT
                         │
                         └──────────────┐
                                        │
                                        ▼
                              CONTINUOUS OPTIMIZATION

The product philosophy
Don't tell the manager everything the cameras saw. Tell the manager what the store needs.

That should guide both the software architecture and the slides.
39. Recommended slide architecture from this master plan
For a strong competition pitch, I would compress this into roughly 9 slides:
1. Problem — Retail cameras see everything, but managers still guess.
2. Insight — We don't track customers. We track the store.
3. StoreFlow concept — Waze for the inside of a store.
4. How it works — CCTV → CV → Store Graph → Metrics.
5. Signature intelligence — Friction Score + Bottleneck + Dead Zone + Flow Drop-off.
6. The manager dashboard — what the manager sees and gets.
7. The differentiator — Layout Experiment: Before vs After.
8. Technical architecture + privacy — how it's actually built.
9. Impact / closing — Observe → Diagnose → Prescribe → Verify.
The project itself can then be developed in the same order: CV foundation → store mapping → metrics → intelligence → dashboard → recommendations → experiments.