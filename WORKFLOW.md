@startmermaid
graph TD
    %% Define Styles
    classDef startend fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef process fill:#e1f5fe,stroke:#0277bd,stroke-width:2px;
    classDef decision fill:#fff9c4,stroke:#fbc02d,stroke-width:2px;
    classDef io fill:#e0f2f1,stroke:#00695c,stroke-width:2px;
    classDef database fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;

    %% Login Process
    Start([Start Application]):::startend --> LoginUI[Display Login Window]:::io
    LoginUI --> UserAction{User Action}:::decision
    
    %% Registration Flow
    UserAction -- Click Register --> RegForm[Show Registration Form]:::io
    RegForm --> InputReg[Input: Name, Email, Phone, Password]:::io
    InputReg --> ValidateReg{Validate Inputs}:::decision
    ValidateReg -- Invalid --> ShowRegError[Show Data Error]:::process
    ShowRegError --> InputReg
    ValidateReg -- Valid --> CheckUser{Check User Exists?}:::database
    CheckUser -- Yes --> ShowExistsError[Show 'Email/Phone Exists']:::process
    ShowExistsError --> InputReg
    CheckUser -- No --> CreateAccount[Create Inactive Account in DB]:::database
    CreateAccount --> RegSuccess[Show 'Registration Successful' Dialog]:::io
    RegSuccess --> LoginUI

    %% Login Flow
    UserAction -- Enter Credentials --> InputCreds[Input Email & Password]:::io
    InputCreds --> ConnectDB{DB Connected?}:::decision
    ConnectDB -- No --> ShowConnError[Show Connection Error]:::process
    ShowConnError --> InputCreds
    ConnectDB -- Yes --> Auth{Authenticate User}:::database
    
    Auth -- User Not Found --> ShowUserError[Show 'Email Not Registered']:::process
    ShowUserError --> InputCreds
    
    Auth -- User Inactive --> ShowActiveError[Show 'Account Inactive']:::process
    ShowActiveError --> InputCreds
    
    Auth -- Invalid Password --> ShowPassError[Show 'Invalid Password']:::process
    ShowPassError --> InputCreds
    
    Auth -- Success --> CheckRole{Check User Role}:::decision

    %% Employee Flow
    CheckRole -- Employee --> EmpDash[Open Employee Dashboard]:::io
    EmpDash --> StartCam[Initialize Webcam]:::process
    StartCam --> DetectFace[Detect Face & Landmarks]:::process
    DetectFace --> ExtractFeat[Extract: Blink, Mouth, Eye, Eyebrow]:::process
    ExtractFeat --> RecogEmotion[DeepFace Emotion Recognition]:::process
    RecogEmotion --> CalcStress[Calculate Stress Score]:::process
    CalcStress --> SaveRec[Save Record to MongoDB]:::database
    SaveRec --> UpdateUI[Update Real-Time Graphs & Stats]:::io
    UpdateUI --> CheckStress{Stress Level?}:::decision
    CheckStress -- High/Medium --> TrigAlert[Trigger Alert & Recommendations]:::process
    TrigAlert --> ShowNotif[Show Windows Notification]:::io
    CheckStress -- Low --> Continue[Continue Monitoring]:::process
    
    EmpTab[Employee Tabs]:::io
    EmpDash --> EmpTab
    EmpTab --> TabProfile[Profile: Edit Details]
    EmpTab --> TabHistory[History: View Past Data]
    EmpTab --> TabRecs[Recommendations: View AI Tips]

    %% Manager Flow
    CheckRole -- Manager --> MgrDash[Open Manager Dashboard]:::io
    MgrDash --> FetchEmp[Fetch Assigned Employees from DB]:::database
    FetchEmp --> DispList[Display Employee List]:::io
    DispList --> MonitorSelect{Select Employee}:::decision
    MonitorSelect --> ViewLive[View Live Status & Stress]:::io
    ViewLive --> ViewHist[View Historical Trends]:::io
    MonitorSelect --> ReceiveAlert[Receive Team Stress Alerts]:::io
    
    %% Admin Flow
    CheckRole -- Admin --> AdminPanel[Open Admin Panel]:::io
    AdminPanel --> FetchUsers[Fetch All Users from DB]:::database
    FetchUsers --> ManageUsers[Manage Users Interface]:::io
    
    ManageUsers --> AdminAction{Action?}:::decision
    AdminAction -- Create User --> AddUserForm[Add User Form]:::io
    AddUserForm --> SaveUser[Save New User to DB]:::database
    
    AdminAction -- Edit User --> EditUserForm[Edit User Form]:::io
    EditUserForm --> UpdateUser[Update User in DB]:::database
    
    AdminAction -- Delete User --> ConfirmDel[Confirm Delete]:::io
    ConfirmDel --> DelUser[Remove User from DB]:::database
    
    AdminAction -- Activate User --> SetActive[Set Status = Active]:::database
    
    AdminPanel --> Analytics[View System-Wide Analytics]:::io

@endmermaid
