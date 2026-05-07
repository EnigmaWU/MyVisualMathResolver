# User Stories — MyVisualMathResolver

This document lists all user stories for the project. Each story follows the BDD **As a / I want / So that** format with **Given / When / Then** acceptance criteria.

---

## US-001: Auto-Advance to Next Word During Remote Dictation

**As a** parent who remotely dictates words to my child,
**I want** the agent to automatically detect when my child has almost finished writing the current word and then notify me to proceed to the next one,
**So that** my child doesn't need to manually signal "ready" each time, and the dictation process becomes smoother and more efficient, just like when I watch them write in person.

---

### Acceptance Criteria

#### Scenario 1: Agent detects near-completion and notifies the parent to advance

- **Given** the parent has started a remote dictation session and spoken a word to the child
- **When** the agent observes that the child has written all (or nearly all) characters of the current word
- **Then** the agent notifies the parent that the child is ready for the next word, without waiting for any manual "ready" signal from the child

#### Scenario 2: Child is still mid-word — agent waits

- **Given** a word has been dictated and the child is still writing
- **When** only part of the word is detected on the writing surface
- **Then** the agent does not advance and continues to monitor until near-completion is detected

#### Scenario 3: Child pauses briefly — agent uses a grace period before advancing

- **Given** the agent detects near-completion of the current word
- **When** the child's pen has been idle for a configurable grace-period (e.g., 2 seconds)
- **Then** the agent treats the pause as completion and notifies the parent to dictate the next word

#### Scenario 4: Child corrects a character after near-completion is detected

- **Given** the agent has tentatively detected near-completion
- **When** the child resumes writing (e.g., fixing a stroke) within the grace period
- **Then** the agent resets its near-completion countdown and waits again

#### Scenario 5: Last word in the dictation list is completed

- **Given** the agent detects near-completion of the final word
- **When** the grace period elapses
- **Then** the agent announces that the dictation session is complete rather than advancing to a next word

#### Scenario 6: Session is paused by the parent mid-dictation

- **Given** a dictation session is in progress and auto-advance is enabled
- **When** the parent explicitly pauses the session
- **Then** the agent stops auto-advancing and waits for the parent to resume, even if near-completion is detected

#### Scenario 7: Agent notifies parent that the next word should start immediately

- **Given** the agent has detected that the child has completed the current word
- **When** the grace period elapses
- **Then** the agent sends a real-time notification to the parent (e.g., a push alert or in-app prompt) signalling that it is time to dictate the next word, so the parent can speak it at the right moment

---

### Notes

- "Near-completion" threshold (e.g., ≥ 80% of expected strokes/characters) should be configurable.
- Grace period before auto-advance should be configurable (default: 2 seconds).
- Auto-advance behavior should be toggleable; some parents may prefer manual advance.
- Detection mechanism may rely on real-time camera feed, stylus event stream, or OCR — implementation details are out of scope for this story.
- Open question: how does the agent handle words with optional characters or regional spelling variants?
