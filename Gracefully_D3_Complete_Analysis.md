# **The Selected Use Cases**

1. Create a science plan  
2. Test a science plan  
3. Submit a science plan  
4. Create an observing program  
5. Access collected astronomical data  

---

## Class diagram
```mermaid
classDiagram
        class User {
          -userID: int
          -firstName: String
          -lastName: String
          -role: String
          -email: String
          -username: String
          -password: String
          -login()
        }

        class Astronomer {
          -astronomerID: int
          -specialization: String
        }

        class ScienceObserver {
          -scienceObserverID: int
        }

        class sciencePlan{
          -planID: int
          -title: String
          -description: String
          -status: String
          -createdDate: String
          -validatedDate: String
          -observedObject: String
          -cost: double
          -objective: String
          -schedule: String
          -createSciencePlan(details)
          -modifyOrEditPlan(details)
          -savePlan(planID)
          -loadPlan(planID)
          -prepareForSubmitPlan(planID)
          -checkParameter(details)
          -confirmSubmitPlan(planID)
          -requestApproval(planID)
          -addSchedule(planID,datetime)
          -getAllPlan()
          -validateDetail()
        }

        class SchedulingRequest{
          -planID: int
          -executeTime: datetime
          -createRequest(planID,datetime)
        }

        class observingProgram{
          -programID: int
          -planID: int
          -location: String
          -status: String
          -dataCollected: String
          -telescopeID: int
          -filterType: String
          -lightControl: double
          -lens: String
          -fillInProgram(programID,planID,status,dataCollected,observingDetail)
          -saveProgram(programID)
          -checkTelescopeReq(detail)
          -createProgram(programID)
          -getAllProgram()
          -confirmCreateProgram(programID)
        }

        class astronomicalData{
          -dataID: int
          -programID: int
          -planID: int
          -dataType: String
          -imgURL: String
          -searchFilter(dataID,programID)
          -selectData(desiredFile)
          -displayData(details)  
          -displayFile(details)      
          -downloadFile(fileURL)
        }

        class Feature{
          -featureType: String
          -featureDescription: String
          -selectFeature(featureType)
        }

        class Template{
          -templateID: int
          -templateTitle: String
          -templateObjective: String
          -selectTemplete(templateID)
          -enterParameter(parameter)
          -fillInDetails(ID,title,description,objective)
          -submitTemplate()
        }

        class virtualTelescope{
          -testedPlanID: int
          -testedStatus: String
          -testPlan(planID)
          -checkParameter(details)
          -startSimulation(planID)
          -processPlan(planID)
          -displayResult(conflict, predicted coverage, warning, results)
          -reviewResult(acceptableStatus)
        }

        User <|-- Astronomer
        User <|-- Science Observer
        Astronomer "1..*" -- "1" Template : select
        Astronomer "1..*" -- "0..*" sciencePlan : create
        sciencePlan "1" -- "1" virtualTelescope: contain
        Astronomer "1..*" -- "0..*" virtualTelescope: test
        Template "1" -- "1..*" sciencePlan : fill
        ScienceObserver "1..*" -- "0..*" sciencePlan : execute
        Astronomer "1" -- "1" Feature : select
        Astronomer "1" -- "0..*" astronomicalData : access
        sciencePlan "0..*" -- "0..*" observingProgram : contain
        ScienceObserver "1..*" -- "0..*" observingProgram : create
        sciencePlan "1" -- "1" astronomicalData : contain
        SchedulingRequest "1" -- "0..*" sciencePlan : has
      
```      

       
     

### **1. Create a Science Plan**

| **Use Case Name** | Create a Science Plan |
| ----- | ----- |
| **ID** | UC001 |
| **Importance Level** | High |
| **Primary Actor** | Astronomer |
| **Use Case Type** | Essential |
| **Stakeholders and Interests** | Astronomer: Wants to create an observation plan.<br>Research Institution: Uses structured plans to schedule telescope time.<br>System Administrator: Protects data quality and security. |
| **Brief Description** | The Astronomer prepares a science plan that outlines the observations to be made. |
| **Preconditions** | The Astronomer is signed in and allowed to create plans. |
| **Trigger** | The Astronomer starts a new plan. |
| **Type** | User-initiated |
| **Relationship** | Association: Astronomer<br>Extend: –<br>Include: –<br>Generalization: – |
| **Normal Flow of Events** | 1. The Astronomer selects “Create New Science Plan” feature.<br>2. The system shows templates to enter observation parameters.<br>3. The Astronomer selects the template.<br>4. The Astronomer fills in the required details.<br>5. The system validates the information.<br>6.If all details are valid, the Astronomer creates the science plan.<br>7. The system displays the plan creation confirmation and plan details.<br>8. The Astronomer modifies the plan if needed.<br>9. The Astronomer saves the plan.<br>10. The system stores the science plan successfully. |
| **Subflow** | 5a. If all information passes validation, the system continues to step 6.<br>5b. If details are missing or invalid, the system highlights the issues and returns to Step 4 for correction.<br>8a. If astronomer don't need to modify the plan, continues to step 9.<br>8b. If astonomer want to modify the plan, the astronomer modify before continues to step 9. <br>9a. If the plan is saved successfully, the system confirms and stores it in the database. |
| **Alternate / Exceptional Flow** | 9b. If an error occurs during saving, the system alerts the Astronomer and requests them to try saving again. |
| **Postconditions** | The Science Plan is created successfully and ready to proceed.<br>The Science Plan is stored and can be scheduled later. |

## Activity Diagram
```mermaid
---
title: Create a Science Plan (UC001)
---
stateDiagram-v2
state "Astronomer selects Create New Science Plan feature" as SelectSciencePlan
state "System shows templates to enter observation parameters" as ShowTemplate
state "Astronomer selects the template" as SelectTemplate
state "Astronomer fills in the required details" as FillDetails
state "System validates the information" as Validate
state "Astronomer creates the science plan" as CreatePlan
state "System highlights invalid/missing details" as Highlights
state "System displays the plan creation confirmation and plan details." as Confirmation
state "Astronomer modifies the plan if needed" as ModifyIfneeded
state "Astronomer saves the plan" as SavePlan
state "System stores the plan" as SaveSuccess
state "System alerts the Astronomer and asks to try again" as SaveError
state "Astronomer modify the plan" as  Needmodify
state ValidateChoice <<choice>>
state SaveChoice <<choice>>
state ModifyChoice <<choice>>
  [*] --> SelectSciencePlan
  SelectSciencePlan --> ShowTemplate
  ShowTemplate --> SelectTemplate
  SelectTemplate --> FillDetails
  FillDetails --> Validate
  Validate --> ValidateChoice
  ValidateChoice --> CreatePlan : all details valid
  ValidateChoice --> Highlights : missing or invalid details
  Highlights --> FillDetails
  CreatePlan --> Confirmation
  Confirmation --> ModifyIfneeded
  ModifyIfneeded --> ModifyChoice
  ModifyChoice --> SavePlan: not need
  ModifyChoice --> Needmodify: Need
  Needmodify --> SavePlan
  SavePlan --> SaveChoice
  SaveChoice --> SaveSuccess: saving successful
  SaveChoice --> SaveError: saving error
  SaveError --> SavePlan
  SaveSuccess --> [*]
```

## Sequence Diagram
```mermaid
---
title: Create a Science Plan (UC001)
---
sequenceDiagram
    actor Astronomer
    participant aFeature
    Astronomer ->> aFeature: selectFeature(createSciencePlan)
    activate Astronomer
    activate aFeature
    aFeature -->> Astronomer: CompletedselectedMessage
    deactivate aFeature
    participant aTemplate
    activate aTemplate
    Astronomer ->> aTemplate: selectTemplete(templateID)
    aTemplate -->> Astronomer: templetePopUp
    loop Retry until valid detail
        Astronomer ->> aTemplate: fillInDetails(ID,title,description,objective)
        aTemplate ->> aSciencePlan: validateDetail()
        aSciencePlan -->> aTemplate: validatedResultMessage
        alt all details valid
            aTemplate -->> Astronomer: completeFillInMessage   
        else missing or invalid details
            aTemplate -->> Astronomer: InvalidFillInDetails
            deactivate aTemplate
        end
    end
    Astronomer ->> aSciencePlan: createSciencePlan(details)
    activate aSciencePlan
    aSciencePlan -->> Astronomer: ConfirmationAndPlanDetailsMessage
    alt plan needs modify
        Astronomer ->> aSciencePlan: modifyOrEditPlan(details)
        aSciencePlan -->> Astronomer: CompletedModifyMessage
    end 
    loop retry until saving successful
        Astronomer ->> aSciencePlan: savePlan(planID)
        alt saving successful
            aSciencePlan -->> Astronomer: saveConfirmationMessage
        else saving error
            aSciencePlan -->> Astronomer: saveErrorMessage
        end
    end
    deactivate Astronomer
    deactivate aSciencePlan
```

---

### **2. Test a Science Plan**

| **Use Case Name** | Test a Science Plan |
| ----- | ----- |
| **ID** | UC002 |
| **Importance Level** | High |
| **Primary Actor** | Astronomer |
| **Use Case Type** | Essential |
| **Stakeholders and Interests** | Astronomer: Wants to check feasibility and expected results before submission.<br>Science Observer: Prefers tested plans to simplify validation. |
| **Brief Description** | The Astronomer simulates the plan to verify accuracy and expected observation flow before submission. |
| **Preconditions** | The Astronomer has already created and saved a draft plan with required details. |
| **Trigger** | The Astronomer clicks on “Test a Plan” to check feasibility. |
| **Type** | External |
| **Relationship** | Association: Astronomer<br>Include: Operate the Interactive Observing (Virtual Telescope)<br>Extend: –<br>Generalization: – |
| **Normal Flow of Events** | 1. The Astronomer selects the feature to test the saved plan.<br>2. The Astronomer selects plan to test.<br>3. The system loads the saved plan into the virtual telescope environment.<br>4. The system checks that all required parameters are present.<br>5. The Astronomer starts the simulation run.<br>6. The system processes the plan and checks for inconsistencies.<br>7. The system displays conflicts, predicted coverage, warnings, and results.<br>8. The Astronomer review the result.<br>9. The system shows tested completed message that the plan is ready for submission. |
| **Subflow** | 4a. If all required parameters are present, continue to Step 5.<br>4b. If parameters are missing or invalid, the system highlights the issues and returns to Step a after the Astronomer edits and saves the plan.<br>8a. If the results are acceptable, the system shows test completed message that the plan is ready for submission.<br>8b. If the results are not acceptable, the Astronomer edit the plan’s parameters and returns to Step 5.|
| **Alternate / Exceptional Flow** |  |
| **Postconditions** | The Astronomer has a verified and improved version of the plan ready for submission. |

## Activity Diagram
```mermaid
---
title: Test a Science Plan (UC002)
---
stateDiagram-v2
SelectTest: The Astronomer selects the test plan feature 
SelectTestplan: Astronomer selects plan to test
LoadPlan: System loads the saved plan into the virtual telescope environment

CheckParameters: System checks that all required parameters are present
StartSimulation: Astronomer starts the simulation run
ProcessPlan: System processes the plan and checks for inconsistencies
DisplayResults: System displays conflicts, predicted coverage, warnings, and results
ReviewResult: Astronomer reviews the result. 

Adjust: Astronomer edits the plan
%% RerunSimulation: Astronomer re-runs the simulation
HighlightInvalid: System highlights issues
EditPlan: Astronomer edits and saves the plan

state ParamChoice <<choice>>
state AcceptChoice <<choice>>
[*] --> SelectTest
SelectTest --> SelectTestplan
SelectTestplan --> LoadPlan
LoadPlan --> CheckParameters
CheckParameters --> ParamChoice
ParamChoice --> StartSimulation: all parameters present
ParamChoice --> HighlightInvalid: missing/invalid parameters
HighlightInvalid --> EditPlan
EditPlan --> CheckParameters
StartSimulation --> ProcessPlan
ProcessPlan --> DisplayResults
DisplayResults --> ReviewResult



Accepted: The system shows tested completed message that the plan is ready for submission

ReviewResult --> AcceptChoice
AcceptChoice --> Accepted: acceptable      
Accepted --> [*]
AcceptChoice --> Adjust: not acceptable    
Adjust --> StartSimulation
```

## Sequence Diagram
```mermaid
---
title: Test a Science Plan (UC002)
---
sequenceDiagram
    actor Astronomer
    participant aFeature
    Astronomer ->> aFeature: selectFeature(testSciencePlan)
    activate Astronomer
    activate aFeature
    aFeature -->> Astronomer: CompletedselectedMessage
    deactivate aFeature
    participant aVirtualTelescope
    participant aSciencePlan
    Astronomer ->> aVirtualTelescope : testPlan(planID)
    activate aVirtualTelescope
    aVirtualTelescope -->> Astronomer : confirmedPlanMessage
    aVirtualTelescope ->> aSciencePlan : loadPlan(planID)
    activate aSciencePlan
    aSciencePlan -->> aVirtualTelescope : loadedResult
 

    loop retry check untill all parameters present

        aVirtualTelescope ->> aVirtualTelescope : checkParameter(details)
        alt all parameters present
            aVirtualTelescope -->> Astronomer : ReadyToSimulateMessage
        else missing or invalid parameter
            aVirtualTelescope -->> Astronomer : invalidParameterDetails
            Astronomer ->> aSciencePlan : modifyOrEditPlan(planID)
            aSciencePlan -->> Astronomer: CompletedEditMessage
        end
    end
    loop resimulate until it's acceptable
        Astronomer ->> aVirtualTelescope : startSimulation(planID)
        aVirtualTelescope ->> aVirtualTelescope : processPlan(planID)
        aVirtualTelescope -->> Astronomer : displayResultMessage
        Astronomer ->> aVirtualTelescope : reviewResult(acceptableStatus)
        alt acceptable
            aVirtualTelescope -->> Astronomer : CompletedTestMessage
        else not acceptable 
            aVirtualTelescope -->> Astronomer : AcknowledgeMessage
            Astronomer ->> aSciencePlan : modifyOrEditPlan(planID)
            aSciencePlan -->> Astronomer: CompletedEditMessage
               
        end
    end  
   
    deactivate Astronomer
    deactivate aVirtualTelescope  
```





---

### **3. Submit a Science Plan**

| **Use Case Name** | Submit a Science Plan |
| ----- | ----- |
| **ID** | UC003 |
| **Importance Level** | High |
| **Primary Actor** | Astronomer |
| **Use Case Type** | Essential |
| **Stakeholders and Interests** | Astronomer: Wants the plan to be submitted so it can be executed on the telescope.<br>Research Institution: Uses submitted plans to schedule observations. |
| **Brief Description** | The Astronomer submits a validated science plan so it can be scheduled for execution. |
| **Preconditions** | A validated science plan exists and has been tested. |
| **Trigger** | The Astronomer chooses to submit the plan. |
| **Type** | External |
| **Relationship** | Association: Astronomer<br>Extend: –<br>Include: –<br>Generalization: – |
| **Normal Flow of Events** | 1. The Astronomer select a submit science plan feature.<br>2. The astronomer loads a tested science plan.<br>3. The Astronomer prepare a plan for submission.<br>4. The system checks whether all required approvals are in place.<br> 5. The Astronomer confirms the submission.<br>6. The system creates a scheduling request for the approved plan. |
| **Subflow** | 4a. If all required approvals and data pass validation, continues to step 5<br>4b. If required approvals are missing, the system shows the invalid parameter, let astronomer to modify, and return to step 3.<br>5a. If the submission is successful, continues to step 6 |
| **Alternate / Exceptional Flow** | 5b. If a system error occurs during submission, the system alerts the Astronomer and requests them to try again.|
| **Postconditions** | The plan is submitted and ready for future implementation. |

## Activity Diagram
```mermaid
---
title: Submit a Science Plan (UC003)
---
stateDiagram-v2
SelectPlan: Astronomer selects tested science plan feature
LoadPlan: Astronomer loads a tested science plan
PreparePlan: Astronomer prepare a plan to system for submission
CheckApprovals: System checks whether all required approvals are in place
ObtainApprovals: System issue invalid parameter
ConfirmSubmission: Astronomer confirms submission
%% AcceptSubmission: System confirms the submission plan
Modify: Astronomer modifies the plan
CreateRequest: System creates a scheduling request
SubmissionError: System notifies Astronomer of error details
state ApprovalChoice <<choice>>
state SubmissionChoice <<choice>>
[*] --> SelectPlan
SelectPlan --> LoadPlan
LoadPlan --> PreparePlan
PreparePlan --> CheckApprovals
CheckApprovals --> ApprovalChoice
ApprovalChoice --> ConfirmSubmission: all approvals in place
ApprovalChoice --> ObtainApprovals: approvals missing
ObtainApprovals --> Modify
Modify --> PreparePlan
ConfirmSubmission --> SubmissionChoice
SubmissionChoice --> CreateRequest: successful
CreateRequest --> [*]
SubmissionChoice --> SubmissionError: unsuccessful
SubmissionError --> ConfirmSubmission

```

## Sequence Diagram
```mermaid
---
title: Submit a Science Plan (UC003)
---
sequenceDiagram
    actor Astronomer
    participant aFeature
    Astronomer ->> aFeature: selectFeature(submitSciencePlan)
    activate Astronomer
    activate aFeature
    aFeature -->> Astronomer: CompletedselectedMessage
    deactivate aFeature
    participant aSciencePlan
    activate aSciencePlan
    Astronomer ->> aSciencePlan : loadPlan(planID)
    aSciencePlan -->> Astronomer : loadedResult
    loop recheck the plan until all approved
        Astronomer ->> aSciencePlan : prepareForSubmitPlan(planID)
        aSciencePlan ->> aSciencePlan : checkParameter(details)
        alt all approvals in place
             aSciencePlan -->> Astronomer: CompletedCheckedMessage
        else approvals missing
             aSciencePlan -->> Astronomer: invalidParameterMessage
             Astronomer ->> aSciencePlan : modifyOrEditPlan(planID)
             aSciencePlan -->> Astronomer: CompletedEditMessage
        end
    end
    loop resubmit until successful submission
        Astronomer ->> aSciencePlan : confirmSubmitPlan(planID)
        alt successful submission
                aSciencePlan -->> Astronomer : successfulStatusMessage
        else unsuccessful submission
                aSciencePlan -->> Astronomer : ErrorDetailsMessage
        end
    end
    deactivate aSciencePlan
    Astronomer ->> aSchedulingRequest : createRequest(planID,datetime)
    activate aSchedulingRequest
    aSchedulingRequest -->> Astronomer : completedCreateRequestMessage
    deactivate aSchedulingRequest
    deactivate Astronomer
    
```
    

---

### **4. Create an Observing Program**

| **Use Case Name** | Create an Observing Program |
| ----- | ----- |
| **ID** | UC004 |
| **Importance Level** | High |
| **Primary Actor** | Science Observer |
| **Use Case Type** | Essential |
| **Stakeholders and Interests** | Science Observer – Wants to transform the science plan into an observing program that can be carried out in real practice.<br>Astronomer – Wants to provide data from the science plan to be used correctly in building the observing program. |
| **Brief Description** | The Science Observer creates an observing program from a validated science plan that can be executed on the telescope. |
| **Preconditions** | The user must log in and be authenticated as the authorized Science Observer before accessing the system. |
| **Trigger** | The Science Observer chooses to create an observing program from a validated science plan. |
| **Type** | External |
| **Relationship** | Association: Science Observer<br>Extend: –<br>Include: –<br>Generalization: – |
| **Normal Flow of Events** | 1. The Science Observer selects a create an observing program feature.<br>2. The Science Observer selects a validated science plan to convert it into an observing program.<br>3. The system shows the details of the selected plan and related data sets.<br>4. The Science Observer creates a new observing program.<br>5. The Science Observer sets parameter values (e.g., magnification, light level, and interpupillary distance).<br>6. The Science Observer confirms the creation.<br>7. The system verifies observing program whether the defined parameters match the telescope’s technical requirements.<br>8. The Science Observer saves the observing program if parameters are valid.<br>9. The system displays the confirmed observing program information. |
| **Subflow** | 7a. If parameters are valid, continue to Step 8.<br>7b. If parameters are invalid, the system highlights the issue and returns to Step 5 for corrections.<br>8a. If the saving is successful, the system displays confirmed observing program information. |
| **Alternate / Exceptional Flow** | 8b. If a saving error occurs while creating the observing program, the system alerts the Science Observer and asks them to retry. |
| **Postconditions** | The Science Observer successfully creates and saves the observing program.<br>The confirmed observing program is stored in the system for future implementation. |

## Activity Diagram
```mermaid
---
title: Create an Observing Program (UC004)
---
stateDiagram-v2
CreateObserve:  The Science Observer selects a create an observing program feature.
SelectPlan: Science Observer selects a validated science plan
ShowDetails: System shows details of the selected plan and related data sets
CreateProgram: Science Observer creates a new observing program
SetParameters: Science Observer sets parameter values (e.g., magnification, light level, distance)
ConfirmCreation: Science Observer confirms creation
ValidateParameters: System verifies observing program whether parameters match telescope technical requirements
SaveProgram: Science Observer saves the observing program
DisplayConfirmation: System displays confirmed observing program information
ShowError: System highlights issue and requests correction
RetrySave: System alerts Science Observer to retry saving
state ValidationChoice <<choice>>
state SaveChoice <<choice>>
[*] --> CreateObserve
CreateObserve --> SelectPlan
SelectPlan --> ShowDetails
ShowDetails --> CreateProgram
CreateProgram --> SetParameters
SetParameters --> ConfirmCreation
ConfirmCreation --> ValidateParameters
ValidateParameters --> ValidationChoice
ValidationChoice --> SaveProgram: parameters valid
ValidationChoice --> ShowError: parameters invalid
ShowError --> SetParameters
SaveProgram --> SaveChoice
SaveChoice --> DisplayConfirmation: saving successful
SaveChoice --> RetrySave: saving error
RetrySave --> SaveProgram
DisplayConfirmation --> [*]
```

## Sequence Diagram
```mermaid
---
title: Create an Observing Program (UC004)
---
sequenceDiagram
    actor ScienceObserver
    participant aFeature
    ScienceObserver ->> aFeature: selectFeature(createObservingProgram)
    activate ScienceObserver
    activate aFeature
    aFeature -->> ScienceObserver: CompletedselectedMessage
    deactivate aFeature
    participant aSciencePlan
    ScienceObserver ->> aSciencePlan: loadPlan(planID)
    activate aSciencePlan
    aSciencePlan -->> ScienceObserver : loadedResult
    deactivate aSciencePlan
    participant anObservingProgram
    ScienceObserver ->> anObservingProgram : createProgram(programID)
    anObservingProgram -->> ScienceObserver : completedCreateMessage  
    loop refill-in until all parameter are valid
        activate anObservingProgram
        ScienceObserver ->> anObservingProgram : fillInProgram(programID,planID,status,dataCollected,observingDetail)
        anObservingProgram -->> ScienceObserver : completedFillInMessage
        ScienceObserver ->> anObservingProgram : confirmCreateProgram(programID)
        anObservingProgram ->> anObservingProgram : checkTelescopeReq(detail)
        alt parameters are valid
            anObservingProgram -->> ScienceObserver : ValidedParameterMessage
        else parameters are invalid
            anObservingProgram -->> ScienceObserver : IssueDatailsMessage
        end
    end
    loop resave until it's successful
        ScienceObserver ->> anObservingProgram : saveProgram(programID)
        alt saving is succussful
            anObservingProgram -->> ScienceObserver : SuccessfulSavedProgramMessage(programDetails)
        else saving error
            anObservingProgram -->> ScienceObserver : ErrorAlertMessage
        end
    end
    deactivate anObservingProgram
    deactivate ScienceObserver

```
---

### **5. Access Collected Astronomical Data**

| **Use Case Name** | Access Collected Astronomical Data |
| ----- | ----- |
| **ID** | UC005 |
| **Importance Level** | High |
| **Primary Actor** | Astronomer |
| **Use Case Type** | Essential |
| **Stakeholders and Interests** | Astronomer – Wants to view, download, or analyze collected astronomical data from completed science plans. |
| **Brief Description** | The Astronomer accesses collected observation data to perform analysis. |
| **Preconditions** | The science plan must be completed, and data must already be collected and stored in the system. |
| **Trigger** | The Astronomer accesses archived data from past observations. |
| **Type** | External |
| **Relationship** | Association: Astronomer<br>Extend: –<br>Include: –<br>Generalization: – |
| **Normal Flow of Events** | 1. The Astronomer selects data access feature.<br>2. The Astronomer enters search filters (e.g., dataID, programID).<br>3. The system displays matching datasets.<br>4. The Astronomer selects the desired files.<br>5. System displays file details with the download link.<br>6. The Astronomer downloads the data.|
| **Subflow** | 2a. If matching data is found, continue to step 3. |
| **Alternate / Exceptional Flow** | 2b. If no data is found, the system shows “No results found” and returns to Step 2. |
| **Postconditions** | The selected astronomical data has been successfully downloaded by the Astronomer.<br>The system logs the access event for traceability. |

## Activity Diagram
```mermaid
---
title: Access Collected Astronomical Data (UC005)
---
stateDiagram-v2
[*] --> OpenDataAccess
OpenDataAccess: Astronomer selects the data access feature
EnterFilters: Astronomer enters search filters (e.g., dateID, programID)
OpenDataAccess --> EnterFilters
DisplayFile: System displays file details with the download link
state if_state_data <<choice>>
EnterFilters --> if_state_data
if_state_data --> NoResults : No data found
if_state_data --> DisplayDatasets : Matching data found
NoResults: System displays message 'No results found.'
NoResults --> EnterFilters
DisplayDatasets: System displays matching datasets
SelectFiles: Astronomer selects desired files
DisplayDatasets --> SelectFiles
SelectFiles --> DisplayFile
DisplayFile --> Download




Download: Astronomer downloads the data
Download --> [*]
```



## Sequence Diagram
```mermaid
---
title: Access Collected Astronomical Data (UC005)
---
sequenceDiagram
    actor Astronomer
    participant aFeature
    Astronomer ->> aFeature: selectFeature(AccessCollectedAstronomicalData)
    activate Astronomer
    activate aFeature
    aFeature -->> Astronomer: CompletedselectedMessage
    deactivate aFeature
    participant anAstronomicalData
    loop search and check until data matched
      Astronomer ->> anAstronomicalData : searchFilter(dataID,programID)
      alt matching data found
        anAstronomicalData -->> Astronomer : dataDisplayMessage
      else no data found
        anAstronomicalData -->> Astronomer : NoFoundDatamessage
      end
    end
    Astronomer ->> anAstronomicalData : selectData(desiredFile)
    anAstronomicalData -->> Astronomer : fileDetailsMessage
    Astronomer ->> anAstronomicalData : downloadFile(fileURL)
    anAstronomicalData -->> Astronomer : conpletedDownloadMessage

    
```
    
