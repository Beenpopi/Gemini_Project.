# Steps to Execute Each Use Case in Your Spring Boot App

Note: This repository already contains a fully configured Spring Boot project. You can open it directly in IntelliJ and skip the Spring Initializr setup. Start testing the use cases from Step 5.
## 1. Choose the Spring Boot Generator in IntelliJ

Use IntelliJ's built-in Spring Initializr to generate a Spring Boot
project.
<br><br>
<img width="793" height="709" alt="Screenshot 2568-11-22 at 14 36 33" src="https://github.com/user-attachments/assets/25e63b89-464f-46ae-ba95-c0da8b1348b7" />


## 2. Create `GeminiController.java` and Test the Path

Create the controller file and run the app. Test the endpoint using your browser or Postman.
<br><br>
<img width="1470" height="956" alt="Screenshot 2568-11-22 at 14 44 27" src="https://github.com/user-attachments/assets/a7a6a2cd-d290-41d8-9f17-518591108787" />


## 3. Import Your `gemini` Package Into the Java Folder

Move your custom `gemini` package into `src/main/java`.
<br><br>
<img width="396" height="233" alt="image" src="https://github.com/user-attachments/assets/24fa661d-aadd-489d-8667-675c733f9c53" />


## 4. Add APIs

Add APIs for: 
- Create Science Plan
- Submit Science Plan
- Create Observing Program
<br><br>
<img width="716" height="169" alt="Screenshot 2568-11-23 at 02 45 53" src="https://github.com/user-attachments/assets/51c6e87b-570d-4cd5-b92b-391c59f34afd" />
<br><br>
<img width="842" height="438" alt="Screenshot 2568-11-23 at 02 46 04" src="https://github.com/user-attachments/assets/fbdd9220-11b0-4970-8ee5-8c820a15eb83" />
<br><br>
<img width="871" height="474" alt="Screenshot 2568-11-23 at 02 46 15" src="https://github.com/user-attachments/assets/6c3ad928-a425-4758-abbe-8f41f66b4c53" />


## 5. Test the APIs
After importing the project, you can test each use case directly from the running Spring Boot app.
### 5.1 Start the Spring Boot application
1. Run the ImplementationApplication class
2. Wait until the console shows something like:  Started ImplementationApplication

Use the following URLs:

-   `http://localhost:8080/api/createSciencePlan`
-   `http://localhost:8080/api/submitSciencePlan`
-   `http://localhost:8080/api/createObservingProgram`
-   `http://localhost:8080/api/updateStatus`


The results will be displayed on the screen.

### 5.2 Test Case 1 Create Science Plans 
1. Open `http://localhost:8080/api/createSciencePlan`
2. Back to the console type 1 for the menu
3. Back to the http page, then you will see the result.
   <br><br>
![messageImage_1763800859692](https://github.com/user-attachments/assets/50aa5ef2-2911-4d30-972d-975aa33b0975)
   <br><br>
### 5.3 Test Case 2 Submit Science Plan 
1. Open `http://localhost:8080/api/submitSciencePlan`
2. To select a filter, use URL parameter ?filter=NUMBER
3. To submit, use URL parameter: ?submit=PLAN_NO
4. You can combine filter + submit. For example, /api/submitSciencePlan?filter=2&submit=104
5. Look at the result printed on the screen.
   <br><br>
![messageImage_1763800741068](https://github.com/user-attachments/assets/1f46f85c-4e36-4b93-ab2d-bc7fb2bdc64a)
<br><br>
### 5.4 Test Case 3 Create Observing Program
1. Send a GET request to `http://localhost:8080/api/createObservingProgram`
2. Look at the result printed on the screen.
  <br><br>
  ![messageImage_1763800879461](https://github.com/user-attachments/assets/d1adb263-248f-4a6e-b70f-1c57a3fff0c9)
<br><br>
### 5.5 Test Case 4 Update Science Plan Status
1. Send a GET request to `http://localhost:8080/api/updateStatus?planNo=101`
2. Look at the result printed on the screen.
<br><br>
![messageImage_1763796958468](https://github.com/user-attachments/assets/ac51854a-8c6c-41da-b778-fe99505d0568)
<br><br>
# Pattern Explanation

## Facade Pattern

This system uses the Facade Pattern to simplify interaction with a
large, complex subsystem.

-   The system includes OCS, which communicates with many subsystems
    (GeminiAPI):
    -   Science Plans
    -   Observing Programs
    -   Astronomical Data
    -   Person including Astronomer, and Science Observer
-   The OCS class acts as a single entry point, hiding complexity.
-   It improves:
    -   Simplicity: It hides complex logic and provides one simple interface.
    -   Readability: The code becomes cleaner because everything is accessed through one entry point.
    -   Maintainability: The subsystems are hidden behind the Facade, changes inside the subsystem will not affect the outside code.

The OCS works as a man‑in‑the‑middle between the subsystem and the
user making method calls.
