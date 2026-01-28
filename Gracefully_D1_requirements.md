# Requirement Gathering
Product owner communication url link:<br>
https://chatgpt.com/share/68b7e7c7-5fa4-800e-bf25-07acddd56ca4 <br>
https://chatgpt.com/share/68bbc4ae-6514-8002-864e-d2abeb055997

# System Requirements Specification (Gemini Project)
There are 7 subsystems.

## 1. Science Plan

### Functional Requirements
**Astronomer**
- The system shall allow Astronomers to create a science plan with objectives, targets, timing, telescope site, processing needs.  
- The system shall allow Astronomers to update or withdraw a Science Plan while it is in a saved or submitted state.  
- The system shall allow Astronomers to submit a science plan for validation.  
- The system shall allow Astronomers to save, edit, and resubmit a science plan.
- The system shall allow Astronomers to specify observing goals and priorities without needing to enter low-level technical control details. 

**Science Observer**
- The system shall allow Science Observers to check the feasibility of a Science Plan by confirming that it can be carried out at the site.
The system shall allow Science Observers to validate that the collected data matches the goals set in the Science Plan.
The system shall allow Science Observers to monitor the execution of Science Plans during observing nights.
The system shall allow Science Observers to provide feedback to Astronomers if changes are needed, for example, due to weather conditions or instrument issues.

**Operation Staff**
- The system shall allow Operation Staff to validate the operational feasibility of a Science Plan, making sure it fits the telescope schedule, instruments, and safety rules.
- The system shall allow Operation Staff to schedule and dispatch validated Science Plans into the observing queue for execution.
- The system shall allow Operation Staff to resequence or pause the execution of a Science Plan when required, for example due to weather or technical problems.
- The system shall allow Operation Staff to act as the point of control between the Astronomer’s intent and the telescope’s safe operation.
- The system shall allow Operation staff to view submitted/validated plans in read-only mode for operational awareness.  

### Non-Functional Requirements
- The system shall prevent unauthorized access and must enforce role and privilege levels for creating, editing, and submitting plans.  
- The system shall log important events (such as create, update, submit, validate) with timestamps so the steps can be recreated later.  
- The system shall classify errors as fatal, serious, or warning. They must be time-stamped, traceable to their source, and easy to extract by time, sequence, or component. Alarms shall require explicit handling.  
- The system shall provide user interfaces that reduce operator fatigue and shall give feedback at each task level.  
- The system shall ensure that planning data and databases are accessed only through defined interfaces to keep modules independent.  
- The system shall support at least 50 simultaneous Astronomers working on Science Plans without performance degradation.  
- The system shall save or update a Science Plan in under 2 seconds after Astronomers click ‘Save’ or ‘Submit.’  

---

## 2. Validation & Scheduling

### Functional Requirements
**Astronomer**
- The system shall allow Astronomers to view and download data only from completed science plans.
- The system shall allow Astronomers to see test results, address comments, and resubmit.
- The system shall allow Astronomers to submit a tested plan for review.

**Science Observer**
- The system shall show Science observers the schedule, so they can check if the target matches the time and telescope location.
- The system shall allow Science observers to test a plan with a virtual telescope/interactive observing mode.  
- The system shall allow Science observers to validate or reject a plan and record reasons.  
- The system shall allow Science observers to run conflict/feasibility checks and attach evidence (logs/plots).

**Operation Staff**
- The system shall provide the Operation staff with a scheduler to manage telescope time (queue, service observing, flexible rescheduling).
- The system shall allow Operation staff to provide feasibility/safety notes that inform validation. 

### Non-Functional Requirements
- The system shall support both automatic and interactive validation/test runs. It must allow switching from automatic to interactive when exceptions happen.  
- The system should use available concurrency to improve the ratio of exposure time to available time.  
- If a subsystem is in Engineering or Maintenance mode, it must ignore commands from other systems but shall still make its status visible.  
- The system shall enforce protection against unauthorized access to validation services.  
- The system shall log test outputs, conflicts, and validation decisions so that runs can be reconstructed later.  
- The user interface shall present validation/test results with clear error messages in plain language.  
- The system shall ensure 99.5% availability during telescope scheduling windows to avoid blocking submissions.  

---

## 3. Observing Program

### Functional Requirements
**Astronomer**
- The system shall allow Astronomers to review the observing program and comment before execution.
- The system shall allow Astronomers to create an observing program from their approved science plan.
- The system shall give Astronomers a simple and safe interface to configure observations, monitor progress, and check data quality.
- The system shall allow Astronomers to add notes or instructions for observers when service observing is used.  

**Science Observer**
- The system shall allow Science observers to transform a validated plan into an observing program with telescope/optics settings.  
- The system shall allow Science observers to version, save, and approve the observing program for execution.
- The system shall allow Science observers review and adjust programs before execution.
- The system shall support Science observers acting for the astronomer when needed.

**Operation Staff**
- The system shall allow Operation staff to validate the observing program for operational readiness.
- The system shall allow Operation staff to enable direct control when special conditions occur.

### Non-Functional Requirements
- The system shall ensure that program creation and approval respect the normal operation states (Observing, Stand-by, Maintenance/Test).  
- The system shall design observing program data structures and interfaces to support easy extension and upgrades over years. It must follow industry standards.  
- The system shall ensure that any online databases related to observing programs are accessed only through defined APIs.  
- The system shall control access modes so that only authorized roles can approve or dispatch observing programs, while others may only monitor.  

---

## 4. Telescope Control

### Functional Requirements
**Astronomer**
- The system shall allow Astronomers to monitor execution in read-only observer mode when authorized. 

**Science Observer**
- The system shall allow Science observers to authorize the run mode and monitor progress/quality during execution.  
- The system shall allow Science observers to record run notes and request adjustments from Operation staff.  

**Operation Staff**
- The system shall allow Operation Staff to directly control the telescope and instruments, including issuing commands for slewing, focusing, instrument setup, and guiding.
- The system shall support Operation Staff in responding quickly to unsafe conditions by providing alerts, alarms, and safety interlocks through the user interface.
- The system shall provide Operation Staff with an interface to communicate and advise Astronomers and Science Observers on efficient use of telescope and instruments.
- The system shall allow Operation Staff to access subsystems in Maintenance or Test mode for diagnostics, calibration, and troubleshooting.
- The system shall allow Operation staff to load an approved observing program.  
- The system shall allow Operation staff to run, pause, resume, and stop execution within authorized modes (interactive/queue/service/remote).  
- The system shall allow Operation staff to monitor live status, alarms/interlocks, and operational telemetry.  

### Non-Functional Requirements
- The system shall bring the telescope to a safe state quickly in case of danger.  
- The system should maximize the percentage of viewable time used for exposures, using concurrency and sequencing where possible.  
- The system shall support high-rate engineering logging (up to 200 Hz for short periods and about 1 Hz for long-term) in a common format for later analysis.  

---

## 5. Data Management

### Functional Requirements
**Astronomer**
- The system shall allow Astronomers to list and download datasets after completion.
- The system shall allow Astronomers to review, accept/reject frames, and request reprocessing with new parameters.
- The system shall allow Astronomers to query the archive for their data but not change storage or system settings. 

**Science Observer**
- The system shall allow Science observers to define default processing profiles and approve the final data package.
- The system shall enforce correct formats and parameters for processing.
- The system shall provide near-line tools to check calibration and science exposures and decide if more data is needed.  

**Operation Staff**
- The system shall allow Operation staff to verify capture sessions, storage paths, and metadata tagging during runs.
- The system shall allow Operation staff to update operational tables, manage data transfer, and ensure system safety.
- The system shall allow Operation staff recover safely from faults during data collection.  

**Supporter**
- The system shall allow Supporters to trigger archival pipelines and verify integrity.
- The system shall allow Supporters maintain hardware, software, and databases for schedules, logs, reports, and science data.
- The system shall support upgrades and keep archive and database systems working.

### Non-Functional Requirements
- The system shall save all astronomical data in the Archiving system.  
- Storage and transfer shall use the FITS format.  
- Online access shall be available through STARCAT.  
- The system shall store astronomical data in both the archiver and the data-storage subsystem.  
- The system shall store configuration data, logs, maintenance schedules, and subsystem documents in a commercial relational database.  
- The system shall keep long-term engineering logs in a common format for consistency.  

---

## 6. Configuration & Administration

### Functional Requirements
**Operation Staff**
- The system shall allow Operation staff to supervise telescope operation, track performance, and ensure safety.
- The system shall give Operation staff update access to operation tables.

**Visitor**
- The system shall allow Visitors to connect their instruments through a standardized interface.
- The system shall give Visitors limited functions: check instrument status, run preprogrammed sequences, and adjust offsets and focus.
- The system shall enforce compliance with interface standards for stability and compatibility.
- The system shall prevent Visitors from interfering with the active instrument or telescope operation.

**Supporter**
- The system shall support system generation, configuration changes, and troubleshooting.
- The system shall provide the highest privileges at maintenance/test levels to modify all parts of the system.
- The system shall enforce configuration control rules so upgrades or changes do not disturb ongoing observations.

### Non-Functional Requirements
- The system shall distribute IOC control databases across subsystems and shall keep initialization copies on Gemini disks.  
- The system shall ensure that all access to control electronics is only through VME IOCs using EPICS; no other control interfaces are permitted.  
- The system shall follow relevant engineering practices and regulations, and it must obtain the required safety approvals.  
- The system shall include built-in tests and self-tests.  
- The system should detect about 90% of subsystem failures before they affect observing and should reduce maintenance problems and costs.  

---

## 7. Remote Access & Simulation

### Functional Requirements
**Astronomer**
- The system shall allow Astronomers to plan remotely using a virtual telescope simulator and to monitor sessions (read-only) when authorized.  

**Science Observer**
- The system shall allow Science observers to perform remote validation/tests using simulator backends and to grant/revoke monitoring access.  

**Operation Staff**
- The system shall allow Operation staff to conduct runs with authorized remote oversight while preserving local safety priority.  

### Non-Functional Requirements
- Several user stations may be active simultaneously.  
- Users shall access only authorized parts of the system from any location with full protection.  
- The system shall provide a Gemini observatory simulator that offers standard interfaces for planning, validation, and testing.  
- The system must enforce intrusion prevention and role-based access protection for remote users.  
