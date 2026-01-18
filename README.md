# Event Registration Application – Zoho Creator

## 📌 Project Overview  
This is a low-code Event Registration Application built using Zoho Creator and Deluge scripting.  
The system allows employees to register for events while enforcing automated validations and workflows.

---

## 🔧 Features Implemented  

### 1. Duplicate Registration Prevention  
- Prevents the same employee from registering for the same event twice  
- Shows alert message  
- Automatically removes duplicate entries  

### 2. Closed Event Restriction  
- Blocks registration for events marked as "Closed"  
- Alerts the user  
- Deletes invalid submissions automatically  

### 3. Email Notification  
- Sends automatic confirmation email when a registration is approved  

### 4. Auto Close Event  
- Automatically changes event status to “Closed” when maximum participants limit is reached  

---

## 🛠 Technologies Used  

- Zoho Creator  
- Deluge Script  
- Workflow Automation  

---

## 📜 Deluge Scripts Used  

### Duplicate Check Script
existing = Registration[Select_Event == input.Select_Event && Select_Employee == input.Select_Employee];

if(existing.count() > 1)
{
    delete from Registration[ID == input.ID];
}

---

### Closed Event Script
eventRecord = Event_List[ID == input.Select_Event];

if(eventRecord.Event_Status == "Closed")
{
    delete from Registration[ID == input.ID];
}

---

### Email Notification Script
if(input.Approval_Status == "Approved")
{
    eventRec = Event_List[ID == input.Select_Event];
    empRec = Employee_Directory[ID == input.Select_Employee];

    sendmail
    [
        from : zoho.adminuserid
        to : empRec.Employee_Email
        subject : "Registration Confirmed"
        message : "Hello " + empRec.Employee_Name + ",\n\n" +
                  "Your registration for the event: " + eventRec.Event_Name +
                  " on " + eventRec.Event_Date +
                  " has been confirmed.\n\nThank you."
    ];
}

---

### Auto Close Event Script
approvedCount = Registration[Select_Event == input.Select_Event && Approval_Status == "Approved"].count();

eventRecord = Event_List[ID == input.Select_Event];

if(approvedCount >= eventRecord.Maximum_Participants)
{
    eventRecord.Event_Status = "Closed";
}

---

## 🧪 Testing  

All functionalities were tested successfully with sample data:

- Duplicate registration prevention  
- Closed event validation  
- Email notification  
- Auto event closing  

---

## 👤 Author  

**Ruchitha M R**  
Information Science Engineering Student  
Zoho Creator Developer (Beginner)  
