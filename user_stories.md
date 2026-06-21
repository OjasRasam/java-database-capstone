# User Stories

## Admin User Stories

### Issue 1: Admin Login
**Title:** Admin Login
_As an admin, I want to log into the portal with my username and password, so that I can manage the platform securely._

**Acceptance Criteria:**
1. A secure login page is accessible to administrators.
2. The system validates the username and password against the database.
3. Successful login redirects the admin to the dashboard.

**Priority:** High
**Story Points:** 3
**Notes:**
- Password should be masked during entry.

### Issue 2: Admin Logout
**Title:** Admin Logout
_As an admin, I want to log out of the portal, so that I can protect system access from unauthorized users._

**Acceptance Criteria:**
1. A visible "Log Out" button is accessible from any screen.
2. Clicking log out ends the current session immediately.

**Priority:** High
**Story Points:** 1

### Issue 3: Add Doctors
**Title:** Add New Doctors
_As an admin, I want to add new doctors to the portal, so that they can begin managing their patient appointments._

**Acceptance Criteria:**
1. A form exists requiring the doctor's name, specialization, and credentials.
2. Saving the form successfully creates the profile.

**Priority:** High
**Story Points:** 5

### Issue 4: Delete Doctor Profile
**Title:** Delete Doctor Profile
_As an admin, I want to delete a doctor's profile from the portal, so that I can maintain an accurate registry._

**Acceptance Criteria:**
1. A "Delete" option is available on the doctor's profile management page.
2. A confirmation pop-up appears before permanent deletion.

**Priority:** Medium
**Story Points:** 3

### Issue 5: Run Appointment Usage Report
**Title:** Run Monthly Appointment Usage Report
_As an admin, I want to run a stored procedure in the MySQL CLI to get the number of appointments per month, so that I can track platform usage statistics._

**Acceptance Criteria:**
1. A stored procedure exists in the database.
2. Running the command via MySQL CLI outputs a clean table of total appointment counts.

**Priority:** Medium
**Story Points:** 3

---

## Patient User Stories

### Issue 1: View Doctor List
**Title:** View Doctor List Without Login
_As a patient, I want to view a list of doctors without logging in, so that I can explore my options._

**Acceptance Criteria:**
1. The landing page displays a list of available doctors.
2. Attempting to book an appointment prompts the user to log in.

**Priority:** High
**Story Points:** 3

### Issue 2: Patient Sign Up
**Title:** Patient Sign Up
_As a patient, I want to sign up using my email and password, so that I can create an account._

**Acceptance Criteria:**
1. A registration form is available requesting email and password.
2. Prevent duplicate accounts by checking if the email already exists.

**Priority:** High
**Story Points:** 5

### Issue 3: Patient Login
**Title:** Patient Login
_As a patient, I want to log into the portal, so that I can manage my bookings securely._

**Acceptance Criteria:**
1. A login interface accepts registered email and password.
2. Valid credentials redirect the patient to their dashboard.

**Priority:** High
**Story Points:** 2

### Issue 4: Book an Appointment
**Title:** Book an Appointment
_As a logged-in patient, I want to book an hour-long appointment, so that I can consult with a specific doctor._

**Acceptance Criteria:**
1. The dashboard provides an intuitive booking calendar selector.
2. Confirming the booking updates the database instantly.

**Priority:** High
**Story Points:** 5

### Issue 5: View Upcoming Appointments
**Title:** View Upcoming Appointments
_As a patient, I want to view my upcoming appointments, so that I can prepare for them accordingly._

**Acceptance Criteria:**
1. A dedicated "My Appointments" section is visible on the dashboard.
2. It displays a chronological list of future bookings.

**Priority:** Medium
**Story Points:** 3

---

## Doctor User Stories

### Issue 1: Doctor Login
**Title:** Doctor Login
_As a doctor, I want to log into the portal, so that I can securely manage my appointments._

**Acceptance Criteria:**
1. A dedicated login interface is available for medical staff.
2. Redirects successful logins to the doctor's dashboard view.

**Priority:** High
**Story Points:** 2

### Issue 2: Doctor Logout
**Title:** Doctor Logout
_As a doctor, I want to log out of the portal, so that I can protect my data._

**Acceptance Criteria:**
1. A clear "Log Out" option is readily available on the dashboard.
2. Clicking log out immediately terminates the active session.

**Priority:** High
**Story Points:** 1

### Issue 3: View Appointment Calendar
**Title:** View Appointment Calendar
_As a doctor, I want to view my appointment calendar, so that I can stay organized._

**Acceptance Criteria:**
1. The dashboard displays a calendar interface showing confirmed patient bookings.

**Priority:** High
**Story Points:** 5

### Issue 4: Manage Unavailability
**Title:** Mark Unavailability Slots
_As a doctor, I want to mark my unavailability in the system, so that patients can only see available slots._

**Acceptance Criteria:**
1. A feature exists within the calendar to select specific hours as "Unavailable."
2. Blocked time slots are instantly hidden on the booking page.

**Priority:** High
**Story Points:** 3

### Issue 5: Update Profile Information
**Title:** Update Profile Information
_As a doctor, I want to update my profile with my specialization, so that patients have up-to-date information._

**Acceptance Criteria:**
1. An "Edit Profile" section allows updating phone number and area of expertise.
2. Saving updates the database immediately.

**Priority:** Medium
**Story Points:** 3
